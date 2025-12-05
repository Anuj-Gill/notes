#tech-notes 
# JavaScript Promises & async/await Notes

## What is a Promise?
- A Promise represents a value that will be available in the future.
- It has 3 states:
  - pending → not finished yet
  - fulfilled → success value
  - rejected → error reason

## What are "handlers" in Promises?
- The functions we attach using:
  - `.then()` → success handler
  - `.catch()` → failure handler
  - `.finally()` → always runs
- Even if the Promise resolves before attaching handlers, they still run.

---

## async/await vs Promises
**Question:** Are async/await and Promises the same?  
**Answer:** No. Async/await is just cleaner syntax to work with Promises. Async/await is just syntactical sugar over Promises.

### Promises version
```js
fetchUser()
  .then(user => fetchPosts(user.id))
  .then(posts => console.log(posts))
  .catch(err => console.error(err));
```

### async/await version
```js
async function load() {
  try {
    const user = await fetchUser();
    const posts = await fetchPosts(user.id);
    console.log(posts);
  } catch (err) {
    console.error(err);
  }
}
```

---

## What does `async` do?
- Marks a function so it **always returns a Promise**
- Allows using `await` inside it

Example:
```js
async function f() {
  return 42; // becomes Promise.resolve(42)
}
```

---

## What does `await` do exactly?
- Pauses only the async function
- Lets other code run meanwhile (non-blocking)
- Resumes when the Promise settles
- If the Promise resolves → gives resolved value
- If the Promise rejects → throws error at that line

**Key reminder:** Forgetting `await` gives you a **Promise**, not the result.

---

## Why do we need async before await?
Because await pauses the function and returns a Promise to the outside.  
JS needs an async context to manage that pause/resume state.

---

## Sequential vs Parallel with await
Sequential (slow but dependent):
```js
await step1();
await step2();
```

Parallel (start both together):
```js
const p1 = step1();
const p2 = step2();
await p1;
await p2;
```

---

## Promise Methods (Static)

### Promise.all()
- Wait for all Promises to succeed
- If **any** fails → whole thing fails (fail-fast)

### Promise.allSettled()
- Wait for all Promises
- Each result includes success or failure
- Good for partial results

### Promise.race()
- Return **first** result (success OR failure)
- Good for timeout logic

### Promise.any()
- Return **first successful** result
- Only rejects if everything fails
- Good for fallback success strategies

### Promise.resolve(value)
- Create a fulfilled Promise from a value

### Promise.reject(error)
- Create a rejected Promise

---

## Quick Comparison Table

| Method | Wait for all? | Stops on first reject? | Returns | Use case |
|--------|----------------|----------------------|---------|----------|
| all | Yes | Yes | All values | Need every result |
| allSettled | Yes | No | Status list | Continue even if some failed |
| race | No | Yes | First settled result | Timeout / speed wins |
| any | No | No (unless all fail) | First success | Fallback for success |

---

## One-line Memory Hooks

- Promise: Future value
- async: Returns a Promise + allows await
- await: Pause here until Promise finishes
- all: all or fail
- allSettled: everything finishes, no matter what
- race: fastest result wins (even failure)
- any: fastest success wins (ignore failures)

---

## Mental mapping async/await to Promises
- `await x` ≈ `x.then(...)`
- `try/catch` ≈ `.catch(...)`
- `finally` stays `finally`

---

## Common mistake reminder
If you log something right after calling an async function, it won't wait:
```js
const data = fetchData(); // Promise
console.log(data); // Not the actual data
```
Solution → `await fetchData()` inside async function.

---

## Final Self-check Questions
- Am I returning real data or a Promise?
- Should multiple async tasks run together (parallel) or in order?
- Do I need safe failure handling (allSettled/any)?
- Did I forget an await?

