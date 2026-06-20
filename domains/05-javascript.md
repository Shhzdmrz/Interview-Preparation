# JavaScript Revision Notes

## typeof null

# Question

What is the output?

```javascript
console.log(typeof null);
```

# Senior-Level Answer

The output is `"object"`.

This is a historical bug in JavaScript. `null` represents the intentional absence of a value, but `typeof null` returns `"object"` because of how early JavaScript represented values internally.

Even though `typeof null` returns `"object"`, `null` is not an actual object.

# Example

```javascript
console.log(typeof null);      // "object"
console.log(null === null);    // true
console.log(null == undefined); // true
console.log(null === undefined); // false
```

# 15-Second Version

`typeof null` returns `"object"`. It is a historical JavaScript bug. `null` means intentional empty value, but it is not really an object.

---

## Truthy and Falsy Values

# Question

What is the output?

```javascript
console.log(Boolean([]));
console.log(Boolean({}));
```

# Senior-Level Answer

Both outputs are `true`.

In JavaScript, empty arrays and empty objects are truthy because they are objects, and objects are truthy even when they contain no values.

The main falsy values in JavaScript are `false`, `0`, `-0`, `0n`, `""`, `null`, `undefined`, and `NaN`. Everything else is truthy.

# Example

```javascript
Boolean([]);        // true
Boolean({});        // true
Boolean("");        // false
Boolean(null);      // false
Boolean(undefined); // false
Boolean(0);         // false
```

# 15-Second Version

Empty arrays and empty objects are truthy in JavaScript. The common falsy values are `false`, `0`, empty string, `null`, `undefined`, and `NaN`.

---

## Hoisting

# Question

What is hoisting in JavaScript?

# Senior-Level Answer

Hoisting is JavaScript's behavior where declarations are processed before code execution.

`var` declarations are hoisted and initialized with `undefined`, so they can be accessed before the line where they are declared, but the value will be `undefined`.

`let` and `const` are also hoisted, but they are not initialized until their declaration line is executed. Accessing them before declaration causes a `ReferenceError` because they are in the Temporal Dead Zone.

Function declarations are fully hoisted, so they can be called before they appear in the code.

# Example

```javascript
console.log(a); // undefined
var a = 10;

console.log(b); // ReferenceError
let b = 20;
```

# 15-Second Version

Hoisting means declarations are processed before execution. `var` is hoisted as `undefined`; `let` and `const` are hoisted but stay in the Temporal Dead Zone until declared.

---

## Event Loop

# Question

What is the output?

```javascript
console.log(1);
setTimeout(() => console.log(2), 0);
Promise.resolve().then(() => console.log(3));
console.log(4);
```

# Senior-Level Answer

The output is:

```text
1
4
3
2
```

JavaScript runs synchronous code first. So `1` and `4` are printed first.

After synchronous code finishes, the event loop processes microtasks before macrotasks. Promise callbacks are microtasks, so `3` runs before `setTimeout`.

`setTimeout` is a macrotask, so `2` runs last even though the delay is `0`.

# Example

```javascript
console.log(1); // sync
setTimeout(() => console.log(2), 0); // macrotask
Promise.resolve().then(() => console.log(3)); // microtask
console.log(4); // sync
```

# 15-Second Version

Synchronous code runs first, then microtasks like Promises, then macrotasks like `setTimeout`. So the output is `1, 4, 3, 2`.

---

## Promise vs Observable

# Question

What is the difference between a Promise and an Observable?

# Senior-Level Answer

A Promise represents a single future value. It either resolves once or rejects once.

An Observable represents a stream of values over time. It can emit multiple values and can be cancelled by unsubscribing.

Promises are commonly used for HTTP calls or one-time async operations. Observables are useful for streams such as WebSocket messages, SignalR events, form value changes, or RxJS-based reactive flows.

# Example

```javascript
const promise = fetch('/api/users');
```

A Promise gives one eventual response.

```javascript
const subscription = messages$.subscribe(message => {
  console.log(message);
});

subscription.unsubscribe();
```

An Observable can keep emitting messages until unsubscribed.

# 15-Second Version

A Promise gives one future result. An Observable is a stream that can emit multiple values over time and can be cancelled by unsubscribing.
