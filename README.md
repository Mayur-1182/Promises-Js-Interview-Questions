# Promises & Async/Await Interview Questions

A curated collection of **output-based JavaScript interview questions** focused on:

- Promises
- Async / Await
- Microtasks vs Macrotasks
- Node.js Event Loop (`process.nextTick`, `setImmediate`, etc.)

These questions are designed from **Basic → Expert** level and are commonly asked in frontend and full-stack interviews.

---

## Files in this Repository

| File | Description |
|------|-------------|
| [promise-interview-questions.md](./promise-interview-questions.md) | Deep dive into Promise methods (`Promise.all`, `allSettled`, `race`, `any`, `.then`, `.catch`, `.finally`) |
| [Async_Await_Event_Loop_Questions.md](./Async_Await_Event_Loop_Questions.md) | Async/Await, Event Loop, `setTimeout`, `process.nextTick`, `setImmediate`, and tricky combinations |

---

## Topics Covered

### Promise Methods
- `Promise.all`
- `Promise.allSettled`
- `Promise.race`
- `Promise.any`
- Chaining with `.then()`, `.catch()`, `.finally()`
- Error handling patterns
- Real-world patterns (timeout, retry, parallel vs sequential)

### Async / Await & Event Loop
- Basic to advanced `async/await`
- Microtask vs Macrotask queue
- `process.nextTick` vs Promise microtasks
- `setTimeout` vs `setImmediate`
- Tricky execution order questions
- Common pitfalls (`forEach` + `async`, sequential vs parallel, etc.)

---

## How to Use

1. Try to predict the output **before** running the code.
2. Run the code in browser console or Node.js.
3. Compare your answer and understand *why* the output is in that order.

---

## Quick Event Loop Order (Node.js)

1. Synchronous code
2. `process.nextTick` callbacks
3. Promise microtasks (`.then` / `.catch` / `.finally`)
4. `setTimeout` / `setInterval` (Timers phase)
5. `setImmediate` (Check phase)
6. I/O callbacks

---

## Contributing

Feel free to open issues or pull requests if you want to add more questions or improve explanations.

---

**Happy Interview Prep!**
