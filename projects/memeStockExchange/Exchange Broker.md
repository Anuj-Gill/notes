### in memory storage - Order Book

#### Interface of incoming order
```
enum OrderType {
	Limit = 'limit',
	Market = 'market'
}

enum OrderAction {
	Buy: 'buy',
	Sell: 'sell'
}

interface Order {
	orderId: string,
	price: number,
	quantity: number,
	orderType: OrderType,
	orderAction: OrderAction
}
```

#### Order Books
```
let sellOrderBook: Order[]  = []
let buyOrderBook: Order[] = []
```

#### DB Schema
Orders table:
- id, userId, symbol, price, quantity, orderType, orderAction, status(enum)

#### Flow of order

- User places order from client
- Create entry in the order's table
- If it is a limit order, add it in the orderBook accordingly and do a sort(asc/desc) based on orderAction
- Follow IOC(Immediate or Cancel) approach for market orders