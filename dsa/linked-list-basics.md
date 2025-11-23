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

#### Implementation

```
//implemented linked list in js

class Node {
    constructor(value) {
        this.value = value
        this.next = null
    }
}

class LinkedList {
    constructor() {
        this.head = null
        this.length = 0
    }
    
    addAtStart(value) {
        const newNode = new Node(value);
        newNode.next = this.head
        this.head = newNode
        this.length++
    }
    
    addAtLast(value) {
        const newNode = new Node(value);
        if(!this.head) {
            this.head = newNode
            this.length++
            return
        }
        
        let currNode = this.head
        while(currNode.next) currNode = currNode.next
        currNode.next = newNode
        this.length++
    }
    
    insertAt(index, value) {
        if(index < 0 || index > this.length) return false
        
        if(index === 0) {
            this.addAtStart(value)
            return true
        }
        
        const newNode = new Node(value);
        let currNode = this.head
        for(let i = 0; i < index - 1; i++) {
            currNode = currNode.next
        }
        newNode.next = currNode.next;
        currNode.next = newNode
        this.length++
    }
    
    removeFirst() {
        if(!this.head) return null
        
        const removedValue = this.head.value
        this.head = this.head.next
        this.length--
        return removedValue
    }
    
    removeLast() {
        if(!this.head) return null
        
        if(!this.head.next) {
            return this.removeFirst()
        }
        
        let currNode = this.head;
        let prev = null
        while(currNode.next) {
            prev = currNode
            currNode = currNode.next
        }
        
        prev.next = null
        this.length--
        return currNode.value
    }
    
    removeAt(index) {
        if(index < 0 || index >= this.length) return false
        
        if(index === 0) {
            return this.removeFirst()
        }
        
        let currNode = this.head
        for(let i = 0; i < index - 1; i++) {
            currNode = currNode.next
        }

        currNode.next = currNode.next.next
        this.length--
        return true
    }
    
    print() {
        let curr = this.head
        let list = ""
    
        while (curr) {
          list += curr.value + " -> "
          curr = curr.next
        }
    
        console.log(list + "null")
  }
}

const list = new LinkedList();

// Build the list
list.addAtStart(20)     // 20
list.addAtStart(10)     // 10 -> 20
list.addAtLast(30)      // 10 -> 20 -> 30
list.addAtLast(40)      // 10 -> 20 -> 30 -> 40
list.insertAt(2, 25)    // 10 -> 20 -> 25 -> 30 -> 40

console.log("After insertions:")
list.print()

// Remove first
list.removeFirst()      // removes 10
console.log("After removeFirst:")
list.print()

// Remove last
list.removeLast()       // removes 40
console.log("After removeLast:")
list.print()

// Remove at index
list.removeAt(1)        // removes 25
console.log("After removeAt(1):")
list.print()
```