> In addition to Singly linked list, a DLL has mapping to previous node's as well. So, we can travers in both directions. But in a SLL, we could only travel from head to tail

```
class Node {
    constructor(value) {
        this.value = value
        this.next = null
        this.prev = null
    }
}

class DoubleLinkedList {
    constructor() {
        this.head = null
        this.length = 0
    }
    
    addAtStart(value) {
        const newNode = new Node(value);
        
        if(!this.head) {
            this.head = newNode
            this.length++
            return 
        }
        
        newNode.next = this.head
        this.head.prev = newNode
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
        newNode.prev = currNode
        this.length++
    }
    
    insertAt(index, value) {
        if(index < 0 || index > this.length) return false
        
        if(index === 0) {
            this.addAtStart(value)
            return true
        }
        
        if(index === this.length) {
            this.addAtLast(value)
            return true
        }
        
        const newNode = new Node(value);
        let currNode = this.head
        for(let i = 0; i < index - 1; i++) {
            currNode = currNode.next
        }
        newNode.next = currNode.next;
        newNode.prev = currNode;
        currNode.next.prev = newNode;
        currNode.next = newNode;
        
        this.length++
        return true;
    }
    
    removeFirst() {
        if(!this.head) return null
        
        const removedValue = this.head.value
        
        if(!this.head.next) {
            this.head = null
            this.length = 0;
            return removedValue;
        }
        
        this.head = this.head.next;
        this.head.prev = null;
        this.length--
        return removedValue
    }
    
    removeLast() {
        if(!this.head) return null
        
        if(!this.head.next) {
            return this.removeFirst()
        }
        
        let currNode = this.head;
        
        while(currNode.next) {
            currNode = currNode.next
        }
        
        currNode.prev.next = null
        currNode.prev = null
        this.length--;
        return currNode.value
    }
    
    removeAt(index) {
        if(index < 0 || index >= this.length) return false
        
        if(index === 0) {
            return this.removeFirst()
        }
        
        if(index === this.length - 1) {
            return this.removeLast()
        }
        
        let currNode = this.head
        for(let i = 0; i < index; i++) {
            currNode = currNode.next
        }
        
        curr.prev.next = curr.next;
        curr.next.prev = curr.prev;

        curr.next = null;
        curr.prev = null;
        this.length--
        return true
    }
    
    print() {
        let curr = this.head
        let list = ""
        let revList = ""
        let last = null
    
        while (curr) {
          list += curr.value + " -> "
          last = curr
          curr = curr.next
        }
        
        while (last) {
            revList += last.value + " -> "
            last = last.prev
        }
    
        console.log(list + "null")
        console.log(revList + "null")
  }
}
```