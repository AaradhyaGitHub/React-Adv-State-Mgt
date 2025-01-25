# Understanding useReducer for State Management in React

## Context and State Management

Context can be a crucial feature in React as it allows us to share state across multiple components efficiently.

But what about state management itself?

## Examining Our Current State Management

Currently, our state management involves state-updating functions that are:

- Quite complex and hard to read.
- Always using the function format when updating state.

### Why Do We Use the Function Pattern for State Updates?

In **90% of cases**, managing object or array state requires updating the state based on the **previous state**. This is why we use the **function pattern** when updating state in React.

## Introducing `useReducer`

React provides another state management hook called `useReducer`, which helps us manage state updates in a structured manner.

### What is a Reducer?

A **reducer** is a function that takes one or more complex values and **reduces** them to a simpler one.

**Example:**
Reducing an array of numbers `[5,10,100]` to `115` through addition.

In JavaScript, we use the `.reduce()` method to accomplish this:

```jsx
const totalPrice = items.reduce(
  (acc, item) => acc + item.price * item.quantity,
  0
);
```

- This method is **independent** of React and `useReducer`; it's a **JavaScript array method**.
- We use this method to define our **reducer function**, which we apply to all items in the `items` array.
- It updates `cartTotal` by adding the price of each `item * quantity` in the cart.
- This is an example of a **reducer function**, as it calculates a **single value (total price)** from multiple values (cart items).

### How Does `useReducer` Work for State Management?

The `useReducer` hook follows a similar concept, taking multiple values and reducing them into a simpler state.

#### Steps to Implement `useReducer`:

1. Call `useReducer()`.
2. Declare state and dispatch function:

```jsx
const [shoppingCartState, shoppingCartDispatch] = useReducer();
```

### What Does `useReducer` Provide?

- An **array** with:
  1. The **current state** (`shoppingCartState`)
  2. A **dispatch function** (`shoppingCartDispatch`)
- The **dispatch function** allows us to dispatch actions that will be handled by a **reducer function**.

### What is a Dispatch Function?

A **dispatch function**:

- Sends **actions** to the reducer.
- Triggers the reducer function to compute and return a new state.

## Defining the Reducer Function

Next, we declare a **reducer function** **outside** the component function:

```jsx
function shoppingCartReducer(state, action) {}
```

### Why Declare the Reducer Outside the Component?

- We **do not** want this function to be recreated every time the component re-renders.
- The reducer function **does not** need access to props or other values inside the component function.

### Parameters of the Reducer Function

- The **state** parameter represents the current state.
- The **action** parameter represents the dispatched action.
- React calls this function automatically after an action is dispatched.

The action dispatched via the `dispatch` function will be received as the second parameter (`action`) inside the reducer function.

---

This structured approach ensures that state updates are managed in a **clear, predictable, and maintainable** way!

---

### Seeing it in action

First of all, the reducer function is supposed to return the new updated state but for now, let's just return the state.

```jsx
function shoppingCartReducer(state, action) {
  return state;
}
```

Now, we need to connect this reducer funciton to our useReducer hook.

### Connecting Reducer function to the useReducer hook

- We need to pass a pointer at that useReducer hook as the first argument:

```jsx
const [shoppingCartState, shoppingCartDispatch] =
  useReducer(shoppingCartReducer);
```

- Now, this function will be used anytime we dispatch
- You also want to pass a second parameter because that allows us to set the initial state for the reducer function. If the state has never been updated yet.
- Equivalent to what we do in useState.
- So, let's copy our state as a intial state for the reducer
- So we have the initial state and the reducer function as arguments to useReducer

```jsx
const [shoppingCartState, shoppingCartDispatch] = useReducer(
  shoppingCartReducer,
  {
    items: []
  }
);
```

- That initial state will be received by

```jsx
function shoppingCartReducer(state, action) {
  return state;
}
```

- Now we can use the first value returned by useState i.e `shoppingCartState` in place of `shoppingCart` in `useState`

- Now, we can use that in our Context Value:

  ```jsx
  const ctxValue = {
    items: shoppingCartState.items,
    addItemToCart: handleAddItemToCart,
    updateItemQuantity: handleUpdateCartItemQuantity
  };
  ```
- At this poi