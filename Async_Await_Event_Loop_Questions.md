# Async JavaScript & Node.js Event Loop - Interview Questions
(Output Based | Basic to Expert Level)

### Q1. What is the output?
```js
console.log("start");

setTimeout(() => {
  console.log("timeout");
}, 0);

console.log("end");
```

### Q2. What is the output?
```js
setTimeout(() => console.log(1), 0);
setTimeout(() => console.log(2), 0);
setTimeout(() => console.log(3), 0);

console.log(4);
```

### Q3. What is the output?
```js
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 0);
}
```

### Q4. What is the output?
```js
for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 0);
}
```

### Q5. What is the output?
```js
setTimeout(() => console.log("A"), 100);
setTimeout(() => console.log("B"), 0);
setTimeout(() => console.log("C"), 50);
```

### Q6. What is the output?
```js
console.log("1");

setTimeout(function () {
  console.log("2");
  setTimeout(function () {
    console.log("3");
  }, 0);
}, 0);

console.log("4");
```

### Q7. What is the output?
```js
const p = new Promise((resolve, reject) => {
  console.log("executor");
  resolve("done");
});

console.log("after promise creation");

p.then((val) => console.log(val));

console.log("end");
```

### Q8. What is the output?
```js
Promise.resolve(1)
  .then((x) => x + 1)
  .then((x) => x + 1)
  .then((x) => console.log(x));
```

### Q9. What is the output?
```js
Promise.resolve("start")
  .then((val) => {
    console.log(val);
    return "middle";
  })
  .then((val) => {
    console.log(val);
  })
  .then((val) => {
    console.log(val);
  });
```

### Q10. What is the output?
```js
const p = Promise.resolve(10);

p.then((v) => console.log("then 1:", v));
p.then((v) => console.log("then 2:", v));
p.then((v) => console.log("then 3:", v));
```

### Q11. What is the output?
```js
new Promise((resolve) => {
  resolve(1);
  resolve(2);
  resolve(3);
}).then((val) => console.log(val));
```

### Q12. What is the output?
```js
Promise.resolve(1)
  .then((val) => {
    throw new Error("oops");
  })
  .then((val) => {
    console.log("then:", val);
  })
  .catch((err) => {
    console.log("catch:", err.message);
  })
  .then(() => {
    console.log("after catch");
  });
```

### Q13. What is the output?
```js
console.log("start");

setTimeout(() => console.log("timeout"), 0);

Promise.resolve().then(() => console.log("promise"));

console.log("end");
```

### Q14. What is the output?
```js
console.log("1");

setTimeout(() => console.log("2"), 0);

Promise.resolve()
  .then(() => console.log("3"))
  .then(() => console.log("4"));

console.log("5");
```

### Q15. What is the output?
```js
setTimeout(() => console.log("timeout 1"), 0);

Promise.resolve().then(() => {
  console.log("promise 1");
  setTimeout(() => console.log("timeout 2"), 0);
});

Promise.resolve().then(() => console.log("promise 2"));
```

### Q16. What is the output?
```js
console.log("start");

setTimeout(() => {
  console.log("timeout 1");
  Promise.resolve().then(() => console.log("promise inside timeout"));
}, 0);

Promise.resolve().then(() => {
  console.log("promise 1");
  setTimeout(() => console.log("timeout inside promise"), 0);
});

console.log("end");
```

### Q17. What is the output?
```js
Promise.resolve()
  .then(() => console.log(1))
  .then(() => console.log(2));

Promise.resolve()
  .then(() => console.log(3))
  .then(() => console.log(4));
```

### Q18. What is the output?
```js
setTimeout(() => console.log("A"), 0);

new Promise((resolve) => {
  console.log("B");
  resolve();
}).then(() => console.log("C"));

console.log("D");
```

### Q19. What is the output? (Node.js)
```js
console.log("start");

process.nextTick(() => console.log("nextTick"));

Promise.resolve().then(() => console.log("promise"));

console.log("end");
```

### Q20. What is the output? (Node.js)
```js
process.nextTick(() => console.log("nextTick 1"));
process.nextTick(() => console.log("nextTick 2"));

Promise.resolve().then(() => console.log("promise 1"));
Promise.resolve().then(() => console.log("promise 2"));

setTimeout(() => console.log("timeout"), 0);

console.log("sync");
```

### Q21. What is the output? (Node.js)
```js
process.nextTick(() => {
  console.log("nextTick 1");
  process.nextTick(() => console.log("nextTick 2"));
});

Promise.resolve().then(() => console.log("promise"));
```

### Q22. What is the output? (Node.js)
```js
setImmediate(() => console.log("setImmediate"));

process.nextTick(() => console.log("nextTick"));

Promise.resolve().then(() => console.log("promise"));

setTimeout(() => console.log("setTimeout"), 0);

console.log("sync");
```

### Q23. What is the output? (Node.js)
```js
setTimeout(() => console.log("setTimeout"), 0);
setImmediate(() => console.log("setImmediate"));
```

### Q24. What is the output? (Node.js)
```js
const fs = require("fs");

fs.readFile(__filename, () => {
  setTimeout(() => console.log("setTimeout"), 0);
  setImmediate(() => console.log("setImmediate"));
});
```

### Q25. What is the output? (Node.js)
```js
setImmediate(() => {
  console.log("setImmediate 1");
  process.nextTick(() => console.log("nextTick inside setImmediate"));
  setImmediate(() => console.log("setImmediate 2"));
});

process.nextTick(() => console.log("nextTick 1"));
```

### Q26. What is the output?
```js
async function foo() {
  return 1;
}

foo().then(console.log);
```

### Q27. What is the output?
```js
async function foo() {
  console.log("start");
  await Promise.resolve();
  console.log("end");
}

foo();
console.log("after foo");
```

### Q28. What is the output?
```js
async function foo() {
  console.log(1);
  await null;
  console.log(2);
}

console.log(3);
foo();
console.log(4);
```

### Q29. What is the output?
```js
async function bar() {
  return "bar";
}

async function foo() {
  const result = await bar();
  console.log(result);
}

foo();
console.log("sync");
```

### Q30. What is the output?
```js
async function foo() {
  console.log("foo start");

  await new Promise((resolve) => {
    console.log("inside promise");
    resolve();
  });

  console.log("foo end");
}

console.log("before foo");
foo();
console.log("after foo");
```

### Q31. What is the output?
```js
async function foo() {
  try {
    const result = await Promise.reject("error");
    console.log("result:", result);
  } catch (e) {
    console.log("caught:", e);
  }
}

foo();
```

### Q32. What is the output?
```js
async function one() {
  console.log("one start");
  await two();
  console.log("one end");
}

async function two() {
  console.log("two start");
  await null;
  console.log("two end");
}

one();
console.log("sync");
```

### Q33. What is the output?
```js
const delay = (ms) => new Promise((resolve) => setTimeout(resolve, ms));

async function main() {
  console.log("start");
  await delay(100);
  console.log("after 100ms");
  await delay(50);
  console.log("after 50ms");
}

main();
console.log("sync after main call");
```

### Q34. What is the output?
```js
async function foo() {
  const p = Promise.resolve("value");
  console.log("before await");
  const result = await p;
  console.log("after await:", result);
}

foo();
console.log("outside");
```

### Q35. What is the output?
```js
async function foo() {
  return await Promise.reject(new Error("fail"));
}

async function bar() {
  try {
    await foo();
  } catch (e) {
    console.log("caught in bar:", e.message);
  }
}

bar();
```

### Q36. What is the output?
```js
Promise.all([
  Promise.resolve(1),
  Promise.resolve(2),
  Promise.resolve(3),
]).then(console.log);
```

### Q37. What is the output?
```js
Promise.all([
  Promise.resolve(1),
  Promise.reject("error"),
  Promise.resolve(3),
])
  .then(console.log)
  .catch(console.log);
```

### Q38. What is the output?
```js
Promise.allSettled([
  Promise.resolve(1),
  Promise.reject("error"),
  Promise.resolve(3),
]).then(console.log);
```

### Q39. What is the output?
```js
Promise.race([
  new Promise((resolve) => setTimeout(() => resolve("slow"), 200)),
  new Promise((resolve) => setTimeout(() => resolve("fast"), 100)),
  new Promise((_, reject) => setTimeout(() => reject("error"), 150)),
]).then(console.log).catch(console.log);
```

### Q40. What is the output?
```js
Promise.any([
  Promise.reject("err1"),
  Promise.resolve("success"),
  Promise.reject("err2"),
]).then(console.log).catch(console.log);
```

### Q41. What is the output?
```js
Promise.any([
  Promise.reject("err1"),
  Promise.reject("err2"),
  Promise.reject("err3"),
]).then(console.log).catch((e) => console.log(e.constructor.name, e.errors));
```

### Q42. What is the output?
```js
console.log("start");

async function asyncFunc() {
  console.log("async start");

  await new Promise((resolve) => {
    setTimeout(() => {
      console.log("setTimeout inside promise");
      resolve();
    }, 0);
  });

  console.log("async end");
}

asyncFunc();

setTimeout(() => console.log("outer setTimeout"), 0);

console.log("end");
```

### Q43. What is the output?
```js
async function a() {
  console.log("a1");
  await b();
  console.log("a2");
}

async function b() {
  console.log("b1");
  await c();
  console.log("b2");
}

async function c() {
  console.log("c1");
}

a();
console.log("sync");
```

### Q44. What is the output?
```js
const p1 = new Promise((resolve) => {
  console.log("p1 executor");
  setTimeout(() => resolve("p1"), 100);
});

const p2 = new Promise((resolve) => {
  console.log("p2 executor");
  setTimeout(() => resolve("p2"), 50);
});

Promise.race([p1, p2]).then((result) => console.log("race winner:", result));

console.log("after setup");
```

### Q45. What is the output?
```js
async function foo() {
  console.log(1);

  await Promise.resolve().then(() => console.log(2));

  console.log(3);
}

foo();
console.log(4);
```

### Q46. What is the output?
```js
Promise.resolve()
  .then(() => {
    console.log(1);
    return Promise.resolve(2);
  })
  .then((val) => console.log(val));

Promise.resolve()
  .then(() => console.log(3))
  .then(() => console.log(4));
```

### Q47. What is the output? (This one is VERY tricky)
```js
async function foo() {
  await 1;
  console.log("foo after await 1");
  await 2;
  console.log("foo after await 2");
}

async function bar() {
  await 3;
  console.log("bar after await 3");
}

foo();
bar();
console.log("sync");
```

### Q48. What is the output?
```js
const promise = new Promise((resolve) => {
  console.log("Promise created");

  setTimeout(() => {
    console.log("Resolving...");
    resolve("resolved value");
  }, 0);
});

promise.then((val) => console.log("Then 1:", val));
promise.then((val) => console.log("Then 2:", val));

console.log("Synchronous end");
```

### Q49. What is the output?
```js
async function main() {
  const results = await Promise.all(
    [1, 2, 3].map(async (num) => {
      console.log("processing", num);
      await null;
      console.log("done", num);
      return num * 2;
    })
  );
  console.log("results:", results);
}

main();
console.log("after main");
```

### Q50. What is the output? (Node.js)
```js
process.nextTick(() => console.log("nextTick 1"));

Promise.resolve().then(() => {
  console.log("promise 1");
  process.nextTick(() => console.log("nextTick inside promise"));
});

process.nextTick(() => {
  console.log("nextTick 2");
  Promise.resolve().then(() => console.log("promise inside nextTick"));
});

console.log("sync");
```

### Q51. What is the output? (Node.js)
```js
const { EventEmitter } = require("events");

const emitter = new EventEmitter();

emitter.on("event", () => {
  console.log("listener 1");
});

emitter.on("event", () => {
  console.log("listener 2");
});

console.log("before emit");
emitter.emit("event");
console.log("after emit");
```

### Q52. What is the output? (Node.js)
```js
setImmediate(() => {
  console.log("setImmediate 1");
  process.nextTick(() => console.log("nextTick inside setImmediate 1"));
});

setImmediate(() => {
  console.log("setImmediate 2");
});

process.nextTick(() => console.log("nextTick 1"));
process.nextTick(() => console.log("nextTick 2"));
```

### Q53. What is the output? (Node.js)
```js
async function main() {
  setImmediate(() => console.log("setImmediate"));
  process.nextTick(() => console.log("nextTick"));
  await null;
  console.log("after await");
}

main();
console.log("sync");
```

### Q54. What is the output?
```js
function delay(ms) {
  return new Promise((resolve) => setTimeout(resolve, ms));
}

async function taskA() {
  await delay(200);
  console.log("Task A done");
}

async function taskB() {
  await delay(100);
  console.log("Task B done");
}

async function main() {
  console.log("Start");
  const a = taskA();
  const b = taskB();
  await a;
  await b;
  console.log("All done");
}

main();
```

### Q55. What is the output?
```js
async function serial() {
  const delay = (ms, val) =>
    new Promise((resolve) => setTimeout(() => resolve(val), ms));

  const r1 = await delay(100, "first");
  console.log(r1);
  const r2 = await delay(50, "second");
  console.log(r2);
}

async function parallel() {
  const delay = (ms, val) =>
    new Promise((resolve) => setTimeout(() => resolve(val), ms));

  const [r1, r2] = await Promise.all([delay(100, "first"), delay(50, "second")]);
  console.log(r1);
  console.log(r2);
}

// Which runs faster: serial() or parallel()?
// What is the order of logs for each?
```

### Q56. What is the output?
```js
const p = new Promise((resolve, reject) => {
  reject("error");
  resolve("success");
});

p.then((v) => console.log("resolved:", v)).catch((e) =>
  console.log("rejected:", e)
);
```

### Q57. What is the output?
```js
async function foo() {
  return 42;
}

const result = foo();
console.log(result);
result.then((v) => console.log(v));
```

### Q58. What is the output?
```js
Promise.resolve(
  new Promise((resolve) => {
    setTimeout(() => resolve("inner"), 100);
  })
).then((val) => console.log("resolved with:", val));
```

### Q59. What is the output?
```js
async function foo() {
  try {
    await Promise.reject("reason");
  } catch (e) {
    console.log("caught:", e);
    return "recovered";
  } finally {
    console.log("finally");
  }
}

foo().then((val) => console.log("then:", val));
```

### Q60. What is the output?
```js
function syncThrow() {
  throw new Error("sync error");
}

async function foo() {
  await syncThrow();
}

foo().catch((e) => console.log("caught:", e.message));
```

### Q61. What is the output?
```js
async function foo() {
  console.log(await Promise.resolve(1) + await Promise.resolve(2));
}

foo();
console.log("sync");
```

### Q62. What is the output?
```js
const arr = [1, 2, 3];

arr.forEach(async (item) => {
  await new Promise((resolve) => setTimeout(resolve, item * 100));
  console.log(item);
});

console.log("forEach done");
```

### Q63. What is the output and what is wrong with this pattern?
```js
async function processAll() {
  const items = [1, 2, 3];

  for (const item of items) {
    await new Promise((resolve) => setTimeout(() => resolve(), item * 100));
    console.log("processed:", item);
  }

  console.log("all processed");
}

processAll();
console.log("after processAll call");
```

### Q64. What happens here? (Node.js behavior)
```js
process.on("unhandledRejection", (reason) => {
  console.log("unhandledRejection:", reason);
});

Promise.reject("oops");
```

### Q65. What is the output?
```js
async function foo() {
  throw new Error("async error");
}

foo();

setTimeout(() => console.log("after timeout"), 0);
```

### Q66. What is the output?
```js
async function fetchData() {
  return await fetch("https://invalid-url-that-fails.xyz");
}

fetchData()
  .then((data) => console.log("success:", data))
  .catch((err) => console.log("error caught:", err.message));
```

### Q67. What is the output?
```js
function* gen() {
  console.log("start");
  yield 1;
  console.log("middle");
  yield 2;
  console.log("end");
}

const g = gen();
console.log(g.next());
console.log(g.next());
console.log(g.next());
```

### Q68. What is the output?
```js
async function* asyncGen() {
  yield await Promise.resolve(1);
  yield await Promise.resolve(2);
  yield await Promise.resolve(3);
}

async function main() {
  for await (const val of asyncGen()) {
    console.log(val);
  }
}

main();
console.log("sync after main");
```

---

## Quick Reference: Event Loop Order (Node.js)

1. Synchronous code (call stack)
2. process.nextTick callbacks
3. Promise microtasks (.then / .catch / .finally)
4. setTimeout / setInterval callbacks (timers phase)
5. setImmediate callbacks (check phase)
6. I/O callbacks

Note: process.nextTick fires BEFORE Promise microtasks.  
Both fire before any macrotasks (setTimeout, setImmediate).

---

## Additional Questions

### Question 1
```js
async function getData() {
  return "Hello";
}

const result = getData();
console.log(result);
```

### Question 2
```js
console.log("Start");

const promise = new Promise((resolve) => {
  console.log("Promise executor");
  resolve("Resolved");
});

promise.then((value) => {
  console.log(value);
});

console.log("End");
```

### Question 3
```js
async function foo() {
  console.log(1);
  await console.log(2);
  console.log(3);
}

console.log(4);
foo();
console.log(5);
```

### Question 4
```js
async function first() {
  await Promise.resolve();
  console.log("First");
}

async function second() {
  console.log("Second start");
  await first();
  console.log("Second end");
}

second();
console.log("Global");
```

### Question 5
```js
function delay(ms, value) {
  return new Promise(resolve => {
    setTimeout(() => {
      console.log(value);
      resolve(value);
    }, ms);
  });
}

async function test() {
  console.log("Start");
  
  const result1 = delay(1000, "One");
  const result2 = delay(500, "Two");
  
  console.log("Middle");
  
  await result1;
  await result2;
  
  console.log("End");
}

test();
```

### Question 6
```js
async function risky() {
  throw new Error("Oops!");
  return "Success";
}

async function handle() {
  try {
    const result = await risky();
    console.log(result);
  } catch (error) {
    console.log("Caught:", error.message);
  }
  
  console.log("Finally done");
}

handle();
```

### Question 7
```js
Promise.resolve("Step 1")
  .then((val) => {
    console.log(val);
    return "Step 2";
  })
  .then((val) => {
    console.log(val);
    return new Promise(resolve => {
      setTimeout(() => resolve("Step 3"), 1000);
    });
  })
  .then((val) => {
    console.log(val);
  });

console.log("Immediate");
```

### Question 8
```js
async function async1() {
  console.log("async1 start");
  await async2();
  console.log("async1 end");
}

async function async2() {
  console.log("async2");
}

console.log("script start");

setTimeout(() => {
  console.log("setTimeout");
}, 0);

async1();

new Promise(resolve => {
  console.log("promise1");
  resolve();
}).then(() => {
  console.log("promise2");
});

console.log("script end");
```

### Question 9
```js
const promises = [];

for (let i = 0; i < 3; i++) {
  promises.push(
    new Promise((resolve) => {
      setTimeout(() => {
        console.log(i);
        resolve(i);
      }, 1000 - i * 200);
    })
  );
}

Promise.all(promises).then(() => {
  console.log("All done");
});
```

### Question 10
```js
async function process() {
  try {
    const result = await new Promise((resolve, reject) => {
      reject("Initial rejection");
    }).catch(err => {
      console.log("First catch:", err);
      return "Recovered";
    });
    
    console.log("Result:", result);
    
    await Promise.reject("Another error");
    
  } catch (error) {
    console.log("Second catch:", error);
  }
}

process().then(() => {
  console.log("Process completed");
});
```

### Question 11
```js
async function* asyncGenerator() {
  yield await Promise.resolve(1);
  yield await Promise.resolve(2);
  yield await Promise.resolve(3);
}

(async () => {
  for await (const num of asyncGenerator()) {
    console.log(num);
  }
  console.log("Done");
})();

console.log("Start");
```

### Question 12 (Nested Promise.resolve)
```js
Promise.resolve()
  .then(() => {
    console.log(1);
    return Promise.resolve(2);
  })
  .then((res) => {
    console.log(res);
  });

Promise.resolve()
  .then(() => {
    console.log(3);
  })
  .then(() => {
    console.log(4);
  });

console.log(5);
```

### Question 13 (Promise.race with Rejections)
```js
const p1 = new Promise((resolve, reject) => {
  setTimeout(() => reject("Error 1"), 100);
});

const p2 = new Promise((resolve) => {
  setTimeout(() => resolve("Success 2"), 50);
});

const p3 = new Promise((resolve, reject) => {
  setTimeout(() => reject("Error 3"), 10);
});

Promise.race([p1, p2, p3])
  .then((result) => console.log("Success:", result))
  .catch((error) => console.log("Error:", error));
```

### Question 14 (Async Function Without Await)
```js
async function noAwait() {
  console.log("A");
  return "B";
}

async function withAwait() {
  console.log("C");
  const result = await noAwait();
  console.log(result);
  console.log("D");
}

console.log("E");
withAwait();
console.log("F");
```

### Question 15 (Multiple Catch Blocks)
```js
new Promise((resolve, reject) => {
  reject("First error");
})
  .catch((err) => {
    console.log("Caught 1:", err);
    throw "Second error";
  })
  .then((val) => {
    console.log("Then:", val);
    return "Normal";
  })
  .catch((err) => {
    console.log("Caught 2:", err);
    return "Recovered";
  })
  .then((val) => {
    console.log("Final:", val);
  });
```

### Question 16 (Async Error in Constructor)
```js
class Test {
  constructor() {
    this.init();
  }

  async init() {
    await Promise.reject("Constructor error");
  }
}

async function test() {
  try {
    const t = new Test();
    await t.init();
  } catch (e) {
    console.log("Caught:", e);
  }
}

test();
console.log("Will error be caught?");
```

### Question 17 (Unhandled Promise Rejection Timing)
```js
async function test() {
  Promise.reject("Silent error");
  
  await new Promise(resolve => setTimeout(resolve, 1000));
  
  console.log("After await");
}

test().catch(e => console.log("Outer catch:", e));

setTimeout(() => {
  console.log("Timeout finished");
}, 2000);
```

### Question 18 (Promise.all with Rejections)
```js
const promises = [
  Promise.resolve(1),
  Promise.reject("Error at index 1"),
  Promise.resolve(3),
  Promise.reject("Error at index 3")
];

Promise.all(promises.map(p => p.catch(e => `Caught: ${e}`)))
  .then(results => {
    console.log("Results:", results);
  });

Promise.allSettled(promises)
  .then(results => {
    console.log("Settled:", results.map(r => r.status));
  });
```

### Question 19 (Promise Chain Return Values)
```js
Promise.resolve(1)
  .then((x) => {
    console.log(x);
    return x + 1;
  })
  .then((x) => {
    console.log(x);
    throw x + 1;
  })
  .catch((x) => {
    console.log(x);
    return x + 1;
  })
  .then((x) => {
    console.log(x);
    return Promise.resolve(x + 1);
  })
  .then((x) => {
    console.log(x);
  });
```

### Question 20 (Async Iteration with Errors)
```js
async function* asyncNumbers() {
  yield 1;
  yield Promise.reject("Error in generator");
  yield 3;
}

(async () => {
  try {
    for await (const num of asyncNumbers()) {
      console.log(num);
    }
  } catch (e) {
    console.log("Generator error:", e);
  }
  console.log("Done");
})();
```

### Question 21 (Promise Constructor Anti-pattern)
```js
function badPattern() {
  return new Promise(async (resolve, reject) => {
    try {
      const result = await Promise.resolve("Async result");
      resolve(result.toUpperCase());
    } catch (e) {
      reject(e);
    }
  });
}

badPattern().then(console.log).catch(console.error);
```

### Question 22 (Multiple Awaits vs Promise.all)
```js
async function sequential() {
  console.time("sequential");
  const a = await new Promise(r => setTimeout(() => r("A"), 1000));
  const b = await new Promise(r => setTimeout(() => r("B"), 1000));
  console.timeEnd("sequential");
  return [a, b];
}

async function parallel() {
  console.time("parallel");
  const [a, b] = await Promise.all([
    new Promise(r => setTimeout(() => r("A"), 1000)),
    new Promise(r => setTimeout(() => r("B"), 1000))
  ]);
  console.timeEnd("parallel");
  return [a, b];
}

sequential().then(() => parallel());
```

### Question 23 (Async Functions in Loops)
```js
const arr = [1, 2, 3];

// Method 1
async function method1() {
  arr.forEach(async (num) => {
    await new Promise(r => setTimeout(r, 100));
    console.log("M1:", num);
  });
  console.log("M1 Done");
}

// Method 2
async function method2() {
  for (const num of arr) {
    await new Promise(r => setTimeout(r, 100));
    console.log("M2:", num);
  }
  console.log("M2 Done");
}

method1();
method2();
```

### Question 24 (Rate Limiting with Promises)
```js
class RateLimiter {
  constructor(limit) {
    this.limit = limit;
    this.queue = [];
    this.active = 0;
  }

  async execute(fn) {
    if (this.active >= this.limit) {
      await new Promise(resolve => this.queue.push(resolve));
    }
    
    this.active++;
    try {
      return await fn();
    } finally {
      this.active--;
      if (this.queue.length > 0) {
        this.queue.shift()();
      }
    }
  }
}

const limiter = new RateLimiter(2);

async function task(id, time) {
  console.log(`Start ${id}`);
  await new Promise(r => setTimeout(r, time));
  console.log(`End ${id}`);
  return id;
}

(async () => {
  const promises = [];
  for (let i = 1; i <= 5; i++) {
    promises.push(limiter.execute(() => task(i, 100 * i)));
  }
  
  const results = await Promise.all(promises);
  console.log("All done:", results);
})();
```

### Question 25 (Cancellable Promise Pattern)
```js
function createCancellablePromise(executor) {
  let cancel;
  const promise = new Promise((resolve, reject) => {
    cancel = (reason) => reject(new Error(`Cancelled: ${reason}`));
    executor(resolve, reject);
  });
  
  return { promise, cancel };
}

const { promise, cancel } = createCancellablePromise((resolve) => {
  const timeout = setTimeout(() => {
    console.log("Resolving");
    resolve("Success");
  }, 1000);
  
  // Cleanup on cancellation
  return () => {
    clearTimeout(timeout);
    console.log("Cleaned up");
  };
});

promise
  .then(console.log)
  .catch(e => console.log(e.message));

setTimeout(() => cancel("Too slow"), 500);
```

### Question 26 (Recursive Async Function)
```js
async function recursive(n) {
  if (n <= 0) return 0;
  
  console.log(`Enter: ${n}`);
  const result = await recursive(n - 1);
  console.log(`Exit: ${n}`);
  
  return n + result;
}

recursive(3).then(console.log);
console.log("Started");
```

### Question 27 (Promise vs setTimeout Priority)
```js
setTimeout(() => console.log("Timeout 1"), 0);

Promise.resolve()
  .then(() => {
    console.log("Promise 1");
    setTimeout(() => console.log("Timeout 2"), 0);
  })
  .then(() => console.log("Promise 2"));

setTimeout(() => console.log("Timeout 3"), 0);

queueMicrotask(() => console.log("Microtask"));

console.log("Sync");
```

### Question 28 (Async/Await Implementation)
```js
// What does this async function compile to?
async function example() {
  const a = await Promise.resolve(1);
  const b = await Promise.resolve(2);
  return a + b;
}

// Rough transpiled version (simplified)
function exampleTranspiled() {
  return Promise.resolve(1)
    .then(function(a) {
      return Promise.resolve(2).then(function(b) {
        return a + b;
      });
    });
}

// Which executes faster and why?
```
