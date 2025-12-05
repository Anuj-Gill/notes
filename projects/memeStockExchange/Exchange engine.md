### In-Memory Data Structure Shape

In-memory order book structure follows this shape:

```json
// a single order object (what persist in DB and also attach to nodes)
type Order = {
  id: string;          // "MEME_..."; produced by DB or sequence
  userId?: string;
  symbol: string;      // "MEME"
  side: "buy" | "sell";
  price: number;       // integer cents
  originalQty: number;
  remainingQty: number;
  type: "limit" | "market";
  status: "OPEN" | "PARTIAL" | "FILLED" | "CANCELLED";
  ts: number;          // created_at
};

// linked list node that wraps an Order in-memory
class ListNode {
  order: Order;
  prev: ListNode | null;
  next: ListNode | null;
  constructor(order: Order) { this.order = order; this.prev = null; this.next = null; }
}

// a simple doubly linked list for a price level
class LinkedList {
  head: ListNode | null;
  tail: ListNode | null;
  length: number;
  // pushTail, popHead, remove(node), toArray() methods...
}

// Now the per-side container (bids or asks):
class OrderBookSide {
  isBid: boolean;                       // true for bids (desc), false for asks (asc)
  priceLevels: number[];                // sorted array of price numbers (authoritative order)
  levelMap: Map<number, LinkedList>;    // price -> linked list of ListNode (FIFO)
  orderMap: Map<string, { price: number, node: ListNode }>; // orderId -> pointer to node
}

//Top-level book for a symbol:
class OrderBook {
  symbol: string;
  bids: OrderBookSide;
  asks: OrderBookSide;
  lastTradePrice?: number;
}

```

- `priceLevels`: Sorted array of prices (descending for bids, ascending for asks)  
- `levels`: Map that holds the DLL  for each price  
- `orderMap`: Maps order ID → DLL node for O(1) lookup and removal


#### DB Schema
Orders table:
```
orders(
  id TEXT PRIMARY KEY,            -- "MEME_<ts>_<seq>" or UUID
  user_id TEXT,
  symbol TEXT,
  side TEXT,                      -- 'buy' | 'sell'
  type TEXT,                      -- 'limit' | 'market'
  price BIGINT,                   -- cents (nullable for pure market orders if you prefer)
  original_qty BIGINT,
  remaining_qty BIGINT,
  status TEXT,                    -- 'OPEN'/'PARTIAL'/'FILLED'/'CANCELLED'
  created_at TIMESTAMP,
  seq BIGINT                      -- optional monotonic sequence for strict ordering
);
```

Trades table:
```
trades(
  id BIGSERIAL PRIMARY KEY,
  symbol TEXT,
  buy_order_id TEXT,
  sell_order_id TEXT,
  price BIGINT,
  qty BIGINT,
  created_at TIMESTAMP
);
```
## Order Processing and Matching Flow

### 1. Order Insertion
1. A new order arrives. --> New endpoint
2. Insert the order into the database first.  
   This returns a unique order ID. --> Prisma call from the service
3. Check if the price exists in `priceLevels`.  
   If not, add the price.
4. In `levelsMap`, either:
   - Create a new DLL for that price, or  
   - Use the existing DLL.
5. Create a new node for the order inside the DLL.
6. Add an entry in `orderMap` linking the order ID to the DLL node.

### 2. Matching Logic Trigger
After the insertion in db, trigger the matching flow
Matching flow:
- Get the orderbook of that symbol
- Get the priceLevels array of the opposite side's orderBookSide(if bid, then priceLevels of asks)
- Get the first entry in the array
- Match the orderPrice with the first Entry
- Cases for match(bid order):
	- if orderPrice > arrayFirstPrice: correct match
		- Using the price, get the head of the linked list of that price. From the head, get the quantity
		- Match the quantity with the order quantity. Make the updates in the db as we have the db id's of both of the order's in their order object
		- After db updates, make the updates here in the db structures, by updating the qunatities. We do a db update first because let say the server crashes just after db updation, so we will get the udpated data only when we reintialise the orderBooks and also the user's get's the confirmed order first which is more imp
		- NOTE: Until this point, we haven't added the new order in the data structres , we will add that in the orderbook only if it's complete quantity is not satisfied, basically we are doing the matchin algo first
		- Then remove the node, the linkedlist if emtpy, the entry from the orderMap and priceLevels: ORder -> first remove the orderMap, then the node and then from the priceLevels array
		  
		

### 3. Matching Rules
1. Based on the order type (bid or ask) and class (limit or market), specific matching logic runs.
2. For a **bid (buy)** order:
   - Check the best available ask price.
   - If `bestAsk <= bidPrice`, process the match.
3. For an **ask (sell)** order:
   - Check the best available bid price.
   - If `bestBid >= askPrice`, process the match.

### 4. Executing a Match
1. Reduce quantities on both sides.
2. Create a transaction entry in the database.
3. If an order becomes fully filled, remove it from:
   - The DLL
   - `levelsMap` if the DLL becomes empty
   - `orderMap`

### 5. Market Orders
1. Market orders are stored in the database for recordkeeping.
2. They are **not** stored in in-memory structures.
3. If no suitable opposite-side order exists, the market order is discarded (flushed) after DB insertion.

### 6. Server Startup Initialization
On server startup:
1. Load all incomplete orders from the database.
2. Reconstruct all in-memory structures:
   - `priceLevels`
   - `levelsMap`
   - DLLs for each price
   - `orderMap`
3. The system resumes matching with a fully rebuilt order book.

