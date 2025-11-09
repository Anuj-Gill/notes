#dsa 

**What:**  
A data structure where each element (node) is stored in non-contiguous memory.  
Each node has data and a pointer to the next node, forming a chain.  
It’s an extendable version of a fixed-size array.

**Where it’s used (in DSA):**  
- Used to implement **Stacks**, **Queues**, **Graphs**, **Adjacency Lists**
- Helps in **dynamic memory allocation** and **fast insert/delete operations**

**Real-life / Tech Use Cases:**  
- **Music/Video playlists** (next/previous navigation)  
- **Undo/Redo** in editors  
- **Browser history** (forward/backward links)  
- **Blockchain** (each block links to next)

---

### Linked List vs Dynamic Array

| Feature | Dynamic Array | Linked List |
|----------|----------------|-------------|
| Memory | Contiguous | Non-contiguous |
| Access (`arr[i]`) | O(1) | O(n) |
| Insert/Delete at start/middle | O(n) | O(1) |
| Insert/Delete at end | O(1)* (amortized) | O(n) |
| Memory overhead | Low | High (extra pointer) |

---

### Key Idea: Constant Time Insertion/Deletion (O(1))
- In **arrays**, inserting or deleting means **shifting** elements → slower (O(n))  
- In **linked lists**, you just **change pointers** → no shifting, faster (O(1))  

**Example:**  
To insert after a node:
1. Create new node  
2. Change current node’s pointer to new node  
3. Point new node to next node  

No element shifting needed.

---

### When to Use
- When frequent **insertions/deletions** are required  
- When memory reallocation (resizing arrays) is costly  
- Used internally in **OS scheduling**, **compilers**, **real-time systems**

**When not to use:**  
If you need **fast random access** or **compact memory usage**, prefer dynamic arrays