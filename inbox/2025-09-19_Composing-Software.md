---
date: 2025-09-19
tags:
  -
hubs:
  - "[[javascript]]"
urls:
  -
---
# 2025-09-19_Composing-Software

## Composing Functions
- It's the process of applying a function to the output of another function.

- Every time you write code like this, you've composing functions:
```js
const g = n => n + 1;
const f = n => n * 2;

const doStuff = x => {
    const afterG = g(x);
    const afterF = f(afterG);
    return afterF;
};
doStuff(20);
```

- Every time you write a promis chain, you're composing functions.

```js
const g = n => n + 1;
const f = n => n * 2;

const wait = time => new Promise(
    (resolve, reject) => setTimeout(
        resolve,
        time
    )
);

wait(300)
    .then(() => 20)
    .then(g)
    .then(f)
    .then(value => console.log(valueJ)) // 42
```

- If you're chaining you're composing

When you compose functions  intentionally, you'll do it better.

```js
const g = n => n + 1;
const f = n => n * 2;

const doStuffBetter = x => f(g(x));

doStuffBetter(20); // 42
```

- A common objection to this form is that it's harder to debug. For example, how would we write this using function composition?

```js
const doStuff = x => {
    const afterG = g(x);
    console.log(`after g: ${ afterG }`);
    const afterF = f(afterG);
    console.log(`after f: ${ afterF }`);
    return afterF;
};
doStuff(20); // =>
/*
"after g: 21"
"after f: 42"
*/
```

- Let's abstract that "after f", "after g" logging into a little utility called `trace()`
 
 ```js
const trace = label => value => {
    console.log(`${ label }: ${ value }`);
    return value;
};

const doStuff = x => {
    const afterG = g(x);
    trace('after g')(afterG);
    const afterF = f(afterG);
    trace('after f')(afterF);
    return afterF;
};
```
