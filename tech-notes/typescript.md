
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