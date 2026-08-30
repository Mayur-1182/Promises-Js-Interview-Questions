# Promise Methods — Interview Questions

Output-based JavaScript Promise questions covering `then`, `catch`, `finally`, `Promise.all`, `Promise.allSettled`, `Promise.race`, and `Promise.any` — from basic to expert level.

## Section 1: Promise.all — Basic

**Q1.**
```javascript
Promise.all([1, 2, 3]).then(console.log);
```

**Q2.**
```javascript
Promise.all([
  Promise.resolve("a"),
  Promise.resolve("b"),
  Promise.resolve("c"),
]).then((results) => {
  console.log(results[0]);
  console.log(results[1]);
  console.log(results[2]);
});
```

**Q3.**
```javascript
const p1 = new Promise((resolve) => setTimeout(() => resolve("slow"), 300));
const p2 = new Promise((resolve) => setTimeout(() => resolve("fast"), 100));
const p3 = new Promise((resolve) => setTimeout(() => resolve("medium"), 200));

Promise.all([p1, p2, p3]).then((results) => console.log(results));

// Question: what is the order of results array?
// and approximately how long does it take to resolve?
```

**Q4.**
```javascript
Promise.all([
  Promise.resolve(1),
  42,
  "hello",
  true,
  Promise.resolve(2),
]).then(console.log);
```

**Q5.**
```javascript
Promise.all([]).then(console.log);
```

## Section 2: Promise.all — Rejection Behavior

**Q6.**
```javascript
Promise.all([
  Promise.resolve(1),
  Promise.reject("error"),
  Promise.resolve(3),
])
  .then((v) => console.log("then:", v))
  .catch((e) => console.log("catch:", e));
```

**Q7.**
```javascript
Promise.all([
  new Promise((_, reject) => setTimeout(() => reject("first error"), 100)),
  new Promise((_, reject) => setTimeout(() => reject("second error"), 50)),
  new Promise((resolve) => setTimeout(() => resolve("success"), 200)),
])
  .then((v) => console.log("then:", v))
  .catch((e) => console.log("catch:", e));

// Which error is caught and why?
```

**Q8.**
```javascript
const p1 = Promise.reject("err1");
const p2 = Promise.reject("err2");
const p3 = Promise.reject("err3");

Promise.all([p1, p2, p3]).catch((e) => console.log("caught:", e));

// How many errors are caught?
```

**Q9.**
```javascript
Promise.all([
  Promise.resolve(1),
  new Promise((resolve, reject) => {
    setTimeout(() => resolve(2), 100);
  }),
  new Promise((resolve, reject) => {
    setTimeout(() => reject("failed"), 50);
  }),
])
  .then((v) => console.log("resolved:", v))
  .catch((e) => console.log("rejected:", e));
```

**Q10.** *(Tricky)*
```javascript
async function fetchAll() {
  try {
    const results = await Promise.all([
      Promise.resolve("user"),
      Promise.reject("network error"),
      Promise.resolve("posts"),
    ]);
    console.log("results:", results);
  } catch (e) {
    console.log("error:", e);
    console.log("partial results available?", false);
  }
}

fetchAll();
```

## Section 3: Promise.allSettled

**Q11.**
```javascript
Promise.allSettled([
  Promise.resolve(1),
  Promise.resolve(2),
  Promise.resolve(3),
]).then(console.log);
```

**Q12.**
```javascript
Promise.allSettled([
  Promise.reject("err1"),
  Promise.reject("err2"),
  Promise.reject("err3"),
])
  .then((v) => console.log("then - length:", v.length))
  .catch((e) => console.log("catch:", e));
```

**Q13.**
```javascript
Promise.allSettled([
  Promise.resolve("success"),
  Promise.reject("failure"),
  new Promise((resolve) => setTimeout(() => resolve("delayed"), 100)),
]).then((results) => {
  results.forEach((result, index) => {
    if (result.status === "fulfilled") {
      console.log(`${index}: fulfilled with`, result.value);
    } else {
      console.log(`${index}: rejected with`, result.reason);
    }
  });
});
```

**Q14.**
```javascript
Promise.allSettled([Promise.resolve(1)])
  .then((v) => console.log("then:", v))
  .catch((e) => console.log("catch:", e))
  .finally(() => console.log("finally"));
```

**Q15.** *(Real world pattern)*
```javascript
async function loadDashboard() {
  const [userResult, postsResult, notifResult] = await Promise.allSettled([
    Promise.resolve({ name: "John" }),
    Promise.reject("posts service down"),
    Promise.resolve([1, 2, 3]),
  ]);

  const user = userResult.status === "fulfilled" ? userResult.value : null;
  const posts = postsResult.status === "fulfilled" ? postsResult.value : [];
  const notifs = notifResult.status === "fulfilled" ? notifResult.value : [];

  console.log("user:", user);
  console.log("posts:", posts);
  console.log("notifs:", notifs);
}

loadDashboard();
```

## Section 4: Promise.race

**Q16.**
```javascript
Promise.race([
  new Promise((resolve) => setTimeout(() => resolve("A"), 300)),
  new Promise((resolve) => setTimeout(() => resolve("B"), 100)),
  new Promise((resolve) => setTimeout(() => resolve("C"), 200)),
]).then(console.log);
```

**Q17.**
```javascript
Promise.race([
  new Promise((_, reject) => setTimeout(() => reject("error"), 100)),
  new Promise((resolve) => setTimeout(() => resolve("success"), 200)),
])
  .then((v) => console.log("resolved:", v))
  .catch((e) => console.log("rejected:", e));
```

**Q18.**
```javascript
Promise.race([
  Promise.resolve("instant 1"),
  Promise.resolve("instant 2"),
  Promise.resolve("instant 3"),
]).then(console.log);
```

**Q19.** *(Real world — timeout pattern)*
```javascript
function withTimeout(promise, ms) {
  const timeout = new Promise((_, reject) =>
    setTimeout(() => reject(`timed out after ${ms}ms`), ms)
  );
  return Promise.race([promise, timeout]);
}

const slowAPI = new Promise((resolve) =>
  setTimeout(() => resolve("api response"), 500)
);

withTimeout(slowAPI, 200)
  .then((v) => console.log("success:", v))
  .catch((e) => console.log("error:", e));
```

**Q20.** *(Tricky)*
```javascript
const p1 = new Promise((resolve) => setTimeout(() => resolve("p1"), 100));
const p2 = new Promise((_, reject) => setTimeout(() => reject("p2 err"), 50));
const p3 = new Promise((resolve) => setTimeout(() => resolve("p3"), 200));

Promise.race([p1, p2, p3])
  .then((v) => console.log("then:", v))
  .catch((e) => console.log("catch:", e));

// after race settles, what happens to p1 and p3?
```

## Section 5: Promise.any

**Q21.**
```javascript
Promise.any([
  Promise.reject("err1"),
  Promise.resolve("first success"),
  Promise.resolve("second success"),
]).then(console.log);
```

**Q22.**
```javascript
Promise.any([
  new Promise((_, reject) => setTimeout(() => reject("slow err"), 300)),
  new Promise((resolve) => setTimeout(() => resolve("fast success"), 100)),
  new Promise((_, reject) => setTimeout(() => reject("medium err"), 200)),
]).then(console.log);
```

**Q23.**
```javascript
Promise.any([
  Promise.reject("err1"),
  Promise.reject("err2"),
  Promise.reject("err3"),
])
  .then((v) => console.log("then:", v))
  .catch((e) => {
    console.log("type:", e.constructor.name);
    console.log("errors:", e.errors);
  });
```

**Q24.** *(Real world — fallback pattern)*
```javascript
async function fetchWithFallback() {
  try {
    const result = await Promise.any([
      Promise.reject("primary server down"),
      Promise.reject("secondary server down"),
      Promise.resolve("tertiary server: data"),
    ]);
    console.log("got data:", result);
  } catch (e) {
    console.log("all servers failed:", e.errors);
  }
}

fetchWithFallback();
```

**Q25.** *(Tricky — race vs any)*
```javascript
Promise.any([
  new Promise((_, reject) => setTimeout(() => reject("err"), 50)),
  new Promise((resolve) => setTimeout(() => resolve("success"), 100)),
])
  .then((v) => console.log("any result:", v))
  .catch((e) => console.log("any error:", e));

Promise.race([
  new Promise((_, reject) => setTimeout(() => reject("err"), 50)),
  new Promise((resolve) => setTimeout(() => resolve("success"), 100)),
])
  .then((v) => console.log("race result:", v))
  .catch((e) => console.log("race error:", e));

// What is different between the two outputs?
```

## Section 6: .then() Chaining — Deep

**Q26.**
```javascript
Promise.resolve(1)
  .then((v) => v + 1)
  .then((v) => v * 2)
  .then((v) => v - 1)
  .then(console.log);
```

**Q27.**
```javascript
Promise.resolve(10)
  .then((v) => {
    console.log("step 1:", v);
    return v * 2;
  })
  .then((v) => {
    console.log("step 2:", v);
    return Promise.resolve(v + 5);
  })
  .then((v) => {
    console.log("step 3:", v);
  })
  .then((v) => {
    console.log("step 4:", v);
  });
```

**Q28.** *(Tricky — what does .then return?)*
```javascript
const p = Promise.resolve("start");

const p2 = p.then((v) => {
  console.log("then 1:", v);
  return "middle";
});

const p3 = p2.then((v) => {
  console.log("then 2:", v);
  return "end";
});

p3.then((v) => console.log("then 3:", v));

console.log("p2 is a Promise?", p2 instanceof Promise);
console.log("sync");
```

**Q29.** *(Throwing inside then)*
```javascript
Promise.resolve("start")
  .then((v) => {
    console.log("then 1:", v);
    throw new Error("something broke");
  })
  .then((v) => {
    console.log("then 2:", v);
  })
  .then((v) => {
    console.log("then 3:", v);
  })
  .catch((e) => {
    console.log("catch:", e.message);
    return "recovered";
  })
  .then((v) => {
    console.log("then after catch:", v);
  });
```

**Q30.** *(Return promise inside then — 2 tick trap)*
```javascript
Promise.resolve()
  .then(() => {
    console.log("A");
    return Promise.resolve("B");
  })
  .then((v) => console.log(v));

Promise.resolve()
  .then(() => console.log("C"))
  .then(() => console.log("D"))
  .then(() => console.log("E"));
```

## Section 7: .catch() and .finally() — Deep

**Q31.**
```javascript
Promise.reject("error")
  .catch((e) => {
    console.log("caught:", e);
    return "fixed";
  })
  .then((v) => console.log("then:", v))
  .catch((e) => console.log("second catch:", e));
```

**Q32.**
```javascript
Promise.reject("error")
  .catch((e) => {
    console.log("caught:", e);
    throw new Error("new error from catch");
  })
  .then((v) => console.log("then:", v))
  .catch((e) => console.log("second catch:", e.message));
```

**Q33.**
```javascript
Promise.resolve("value")
  .finally(() => {
    console.log("finally 1");
    return "finally return value";
  })
  .then((v) => console.log("then:", v));

// Does finally change the resolved value?
```

**Q34.**
```javascript
Promise.reject("original error")
  .finally(() => {
    console.log("finally runs");
    return "finally return";
  })
  .catch((e) => console.log("catch:", e));

// Does finally swallow the rejection?
```

**Q35.** *(Tricky — finally throwing)*
```javascript
Promise.resolve("success")
  .finally(() => {
    console.log("finally");
    throw new Error("finally error");
  })
  .then((v) => console.log("then:", v))
  .catch((e) => console.log("catch:", e.message));
```

**Q36.**
```javascript
Promise.reject("err")
  .finally(() => {
    console.log("finally 1");
  })
  .finally(() => {
    console.log("finally 2");
  })
  .catch((e) => console.log("catch:", e));
```

## Section 8: Combining Methods — Expert

**Q37.**
```javascript
async function run() {
  const results = await Promise.allSettled([
    Promise.all([Promise.resolve(1), Promise.resolve(2)]),
    Promise.all([Promise.resolve(3), Promise.reject("inner error")]),
    Promise.resolve(4),
  ]);

  results.forEach((r) => {
    if (r.status === "fulfilled") console.log("ok:", r.value);
    else console.log("fail:", r.reason);
  });
}

run();
```

**Q38.**
```javascript
async function getData() {
  return Promise.race([
    Promise.allSettled([
      Promise.resolve("a"),
      Promise.reject("b"),
    ]),
    new Promise((resolve) => setTimeout(() => resolve("timeout winner"), 0)),
  ]);
}

getData().then(console.log);
```

**Q39.** *(Real world pattern — retry logic)*
```javascript
function attempt(fn, retries) {
  return fn().catch((err) => {
    if (retries <= 0) throw err;
    console.log("retrying... attempts left:", retries - 1);
    return attempt(fn, retries - 1);
  });
}

let count = 0;

attempt(() => {
  count++;
  console.log("attempt number:", count);
  if (count < 3) return Promise.reject("failed");
  return Promise.resolve("success on attempt " + count);
}, 5).then(console.log);
```

**Q40.** *(Real world pattern — parallel with limit)*
```javascript
async function processInBatches() {
  const items = [1, 2, 3, 4, 5];

  const results = await Promise.all(
    items.map(async (item) => {
      await new Promise((resolve) => setTimeout(resolve, item * 10));
      console.log("processed:", item);
      return item * 2;
    })
  );

  console.log("all results:", results);
}

processInBatches();
console.log("processing started");
```

**Q41.** *(Tricky — Promise.all inside async)*
```javascript
async function fetchUser() {
  console.log("fetchUser called");
  await null;
  return { name: "John" };
}

async function fetchPosts() {
  console.log("fetchPosts called");
  await null;
  return ["post1", "post2"];
}

async function main() {
  console.log("main start");
  const [user, posts] = await Promise.all([fetchUser(), fetchPosts()]);
  console.log("user:", user.name);
  console.log("posts:", posts.length);
}

main();
console.log("sync after main");
```

## Section 9: Tricky Edge Cases

**Q42.** *(then with two arguments)*
```javascript
Promise.resolve("value")
  .then(
    (v) => console.log("onFulfilled:", v),
    (e) => console.log("onRejected:", e)
  );

Promise.reject("error")
  .then(
    (v) => console.log("onFulfilled:", v),
    (e) => console.log("onRejected:", e)
  );
```

**Q43.** *(difference between then(null, fn) and catch)*
```javascript
Promise.reject("err")
  .then(() => {
    throw new Error("then error");
  })
  .catch((e) => console.log("catch:", e));

Promise.reject("err")
  .then(
    () => { throw new Error("then error"); },
    (e) => console.log("onRejected:", e)
  )
  .catch((e) => console.log("catch:", e));

// What is different between the two?
```

**Q44.** *(Promise.all with async functions)*
```javascript
async function double(n) {
  return n * 2;
}

async function triple(n) {
  return n * 3;
}

async function main() {
  const results = await Promise.all([double(5), triple(5)]);
  console.log(results);
}

main();
```

**Q45.** *(Unhandled rejection behavior)*
```javascript
async function main() {
  const p1 = Promise.reject("error 1");
  const p2 = Promise.reject("error 2");

  await new Promise((resolve) => setTimeout(resolve, 100));

  try {
    await Promise.all([p1, p2]);
  } catch (e) {
    console.log("caught:", e);
  }
}

main();

// What happens to p2's rejection?
// Is there an unhandled rejection warning?
```

**Q46.** *(Promise chaining vs async/await equivalence)*
```javascript
// Are these two exactly the same?

// Version A - chaining
function versionA() {
  return Promise.resolve(1)
    .then((v) => v + 1)
    .then((v) => v * 2)
    .then((v) => console.log("A result:", v));
}

// Version B - async/await
async function versionB() {
  const v1 = await Promise.resolve(1);
  const v2 = v1 + 1;
  const v3 = v2 * 2;
  console.log("B result:", v3);
}

versionA();
versionB();
console.log("sync");

// What are the outputs and are they in same order?
```

## Section 10: Real World Interview Scenarios

**Q47.** *(Classic interview — what is wrong with this code?)*
```javascript
async function getUserData(userId) {
  const user = await fetch(`/api/users/${userId}`);
  const posts = await fetch(`/api/posts/${userId}`);
  const comments = await fetch(`/api/comments/${userId}`);
  return { user, posts, comments };
}

// What is the problem?
// How do you fix it?
// Write the fixed version.
```

**Q48.** *(What is wrong with this forEach?)*
```javascript
async function processItems(items) {
  items.forEach(async (item) => {
    const result = await processItem(item);
    console.log("done:", result);
  });
  console.log("all done");
}

// Why does "all done" print before the items?
// Give two ways to fix it.
```

**Q49.** *(Classic — sequential vs parallel time)*
```javascript
const delay = (ms, v) => new Promise((r) => setTimeout(() => r(v), ms));

async function sequential() {
  console.time("sequential");
  const a = await delay(100, "a");
  const b = await delay(100, "b");
  const c = await delay(100, "c");
  console.timeEnd("sequential");
  return [a, b, c];
}

async function parallel() {
  console.time("parallel");
  const [a, b, c] = await Promise.all([
    delay(100, "a"),
    delay(100, "b"),
    delay(100, "c"),
  ]);
  console.timeEnd("parallel");
  return [a, b, c];
}

sequential().then(console.log);
parallel().then(console.log);

// Approximately how long does each take?
// What are the results arrays?
```

**Q50.** *(Final boss — error handling chain)*
```javascript
async function step1() {
  console.log("step1 start");
  await null;
  throw new Error("step1 failed");
}

async function step2() {
  console.log("step2 start");
  await null;
  return "step2 done";
}

async function main() {
  const results = await Promise.allSettled([step1(), step2()]);

  for (const result of results) {
    if (result.status === "fulfilled") {
      console.log("success:", result.value);
    } else {
      console.log("failed:", result.reason.message);
    }
  }
}

main();
console.log("sync");
```

## Quick Reference — Promise Methods Comparison

| Method | Resolves When | Rejects When |
|---|---|---|
| `Promise.all` | ALL resolve | ANY one rejects (fast fail) |
| `Promise.allSettled` | ALL settle | Never rejects |
| `Promise.any` | FIRST one resolves | ALL reject (`AggregateError`) |
| `Promise.race` | FIRST one settles | FIRST one rejects |

**Key rules:**
- `.then()` always returns a new promise
- `.catch(fn)` is `.then(undefined, fn)`
- `.finally()` does NOT change the resolved value
- `.finally()` DOES propagate if it throws
- Non-promise values in `Promise.all` are auto-wrapped
- `Promise.all` result order = input order, not resolve order
