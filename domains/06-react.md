# React Revision Notes

## Props vs State

# Question

What is the difference between props and state in React?

# Senior-Level Answer

Props are read-only values passed from a parent component to a child component. They allow data and callbacks to flow from parent to child.

State is data managed inside a component. When state changes, React re-renders the component.

A child component should not modify props directly. If the child needs to trigger a change, the parent should pass a callback function as a prop.

# Example

```jsx
function Parent() {
  const [count, setCount] = useState(0);

  return <Child count={count} onIncrement={() => setCount(count + 1)} />;
}

function Child({ count, onIncrement }) {
  return <button onClick={onIncrement}>{count}</button>;
}
```

`count` is state in the parent and a prop in the child.

# 15-Second Version

Props are read-only inputs passed from parent to child. State is data owned by a component. Changing state re-renders the component; props should not be modified directly by the child.

---

## useRef vs useMemo

# Question

What is the difference between `useRef` and `useMemo`?

# Senior-Level Answer

`useRef` stores a mutable value that persists across renders but does not trigger a re-render when changed. It is commonly used for DOM references, timers, previous values, or values that should survive renders without affecting UI.

`useMemo` memoizes the result of an expensive calculation. It recalculates the value only when its dependencies change.

Use `useRef` when you need to store a value without causing re-render. Use `useMemo` when you want to avoid recalculating an expensive computed value.

# Example

```jsx
const inputRef = useRef(null);

function focusInput() {
  inputRef.current.focus();
}
```

`useRef` is useful for accessing DOM elements.

```jsx
const filteredUsers = useMemo(() => {
  return users.filter(user => user.name.includes(search));
}, [users, search]);
```

`useMemo` is useful for memoizing calculated values.

# 15-Second Version

`useRef` stores a mutable value that does not trigger re-render. `useMemo` caches a computed value and recalculates only when dependencies change.

---

## useMemo vs useCallback

# Question

What is the difference between `useMemo` and `useCallback`?

# Senior-Level Answer

`useMemo` memoizes a computed value.

`useCallback` memoizes a function reference.

Both are performance optimizations and should be used when they solve a real re-render or expensive calculation problem, not everywhere by default.

`useCallback` is useful when passing functions to memoized child components because it prevents unnecessary function reference changes.

# Example

```jsx
const total = useMemo(() => {
  return items.reduce((sum, item) => sum + item.price, 0);
}, [items]);
```

`useMemo` caches a calculated value.

```jsx
const handleClick = useCallback(() => {
  saveOrder(orderId);
}, [orderId]);
```

`useCallback` caches a function reference.

# 15-Second Version

`useMemo` caches a computed value. `useCallback` caches a function reference. Both are performance optimizations and should be used only when needed.

---

## React Keys

# Question

Why does React need a `key` prop when rendering lists?

# Senior-Level Answer

React uses the `key` prop to identify which list items changed, were added, removed, or reordered during reconciliation.

Keys help React preserve component identity and state correctly.

A key should be stable and unique, such as a database ID. Using an array index as a key is risky when the list can be reordered, filtered, inserted into, or deleted from because React may reuse the wrong component state.

# Example

```jsx
users.map(user => (
  <UserRow key={user.id} user={user} />
));
```

This is better than:

```jsx
users.map((user, index) => (
  <UserRow key={index} user={user} />
));
```

because `user.id` remains stable even if the list order changes.

# 15-Second Version

React keys help identify list items during re-rendering. Use stable unique IDs. Avoid array index keys when items can be reordered, inserted, or deleted.

---

## Context API vs Redux

# Question

What is the difference between Context API and Redux?

# Senior-Level Answer

Context API is built into React and is useful for sharing relatively simple global values such as authenticated user, theme, language, tenant, or permissions.

Redux is better for large and complex application state where predictable updates, middleware, debugging tools, time-travel debugging, and strict state management are useful.

Context API can reduce prop drilling, but it does not fully replace Redux for complex applications.

# Example

Use Context API for:

```text
Authenticated user
Theme
Language
Tenant
Permissions
```

Use Redux for:

```text
Complex shopping cart
Large dashboard filters
Cross-page workflow state
Enterprise application state
```

# 15-Second Version

Context API is good for simple shared values like auth user or theme. Redux is better for complex global state with predictable updates, middleware, and better debugging tools.
