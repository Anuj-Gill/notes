
### What?
- JavaScript runs → TypeScript checks before running.  
- It’s about _types as contracts_. TS makes mistakes visible before runtime.

**Inference**: when you initialize, TS figures it out.  
**Annotation**: when you don’t initialize, you must tell TS the type.

Basic type annotations:
```
let username: string = "Gill"
let age: number = 21
let isAdmin: boolean = true
```

`any` turns off type checking. Avoid it unless migrating old JS.

`unknown` —> safer alternative to `any`
You can store anything, but _you must check before using it_.
```
let value: unknown = "hello"

if (typeof value === "string") {
  console.log(value.toUpperCase()) // safe
}
```
If you don’t narrow it, TS won’t let you use `value` as a string.

`never` —> means it never happens(function never finishes at all)
Used when a function **does not return** —> because it throws or loops forever.
Here, control **never reaches** the caller, not even to return `undefined`.
```
function throwError(msg: string): never {
   throw new Error(msg)
}
```

`void`: “function returns nothing useful”
Used when a function **finishes normally** but doesn’t return a value.
Example:
```
function logMessage(msg: string): void {   
console.log(msg)   // function ends, control returns to caller 
}
```
So `void` means:
> “There _is_ a return point, but no value is returned.”

#### Generics
They let you create **reusable components** that work with _any type_ - without losing type safety.
```
function identity<T>(value: T): T {
	return value
}

identity(10) //type: number
identity("hello") //type: string
```
Here `T` is a **type placeholder** that gets replaced by whatever type you pass in.

#### Generic constraints - `extends`
Sometimes you don’t want _any_ type; you want one that at least meets certain requirements.
```
function getLength<T extends { length: number }>(item: T): number {
  return item.length
}

getLength("hello")  // ✅ string has length
getLength([1, 2, 3]) // ✅ array has length
getLength(42)       // ❌ number has no length
```
So `extends` here means **“must have at least this shape.”**

#### Generics in interfaces and classes
```
interface Box<T> {
  value: T
}

const stringBox: Box<string> = { value: "hi" }
const numberBox: Box<number> = { value: 42 }

//Classes
class Store<T> {
  constructor(private items: T[]) {}
  add(item: T) {
    this.items.push(item)
  }
  getAll(): T[] {
    return this.items
  }
}

const stringStore = new Store<string>([])
stringStore.add("apple")

```

#### typeof
 **typeof** used inside a type annotation reads the static type of a value

#### Parameters 
It extracts a tuple of the parameter types
Example:
```
function createClient(a: string, b: string, options?: { debug: boolean }) { /* ... */ }
//then
type Params = Parameters<typeof createClient>
// Params is [string, string, { debug: boolean }?]
```

