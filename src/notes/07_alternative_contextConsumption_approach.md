# Understanding Another Way to Consume Context in React

## Introduction
Before diving deeper into React Context and its usage with the `useContext` hook, let's explore an alternative method of consuming context. While `useContext` is the standard and most commonly used way, there's another approach that exists in React—especially in projects with older codebases.

In addition to the `.Provider` component that is required to wrap components needing access to context, React also provides a `.Consumer` component for every created context. This `.Consumer` component allows us to access context values in a different way.

## Our Current Approach: Using `useContext`
Right now, we use the `useContext` hook to connect a component to a context and make its value available within the component. Here’s how we currently implement it in our `Cart.jsx` component:

```jsx
import { useContext } from "react";
import { CartContext } from "../store/shopping-cart-context.jsx";

export default function Cart({ onUpdateItemQuantity }) {
  const { items } = useContext(CartContext);
  
  const totalPrice = items.reduce(
    (acc, item) => acc + item.price * item.quantity,
    0
  );
  const formattedTotalPrice = `$${totalPrice.toFixed(2)}`;

  return (
    <div id="cart">
      {items.length === 0 && <p>No items in cart!</p>}
      {items.length > 0 && (
        <ul id="cart-items">
          {items.map((item) => {
            const formattedPrice = `$${item.price.toFixed(2)}`;
            return (
              <li key={item.id}>
                <div>
                  <span>{item.name}</span>
                  <span> ({formattedPrice})</span>
                </div>
                <div className="cart-item-actions">
                  <button onClick={() => onUpdateItemQuantity(item.id, -1)}>
                    -
                  </button>
                  <span>{item.quantity}</span>
                  <button onClick={() => onUpdateItemQuantity(item.id, 1)}>
                    +
                  </button>
                </div>
              </li>
            );
          })}
        </ul>
      )}
      <p id="cart-total-price">
        Cart Total: <strong>{formattedTotalPrice}</strong>
      </p>
    </div>
  );
}
```

### Breakdown of `useContext`:
- `useContext(CartContext)`: This connects the component to the `CartContext` and extracts the `items` array from it.
- The component calculates the total price of the cart by iterating over the `items` array using `reduce()`.
- The cart is then displayed dynamically based on whether there are items or not.

## Alternative Approach: Using the `.Consumer` Component
Instead of using `useContext`, we can utilize the `.Consumer` component, which is built into the `CartContext`. The `.Consumer` component is wrapped around JSX code that needs access to the context value.

### Syntax:
The `.Consumer` component follows this structure:

```jsx
<CartContext.Consumer>
  {(contextValue) => {
    return JSX_CODE;
  }}
</CartContext.Consumer>
```

Key points to note:
- The `.Consumer` component does **not** take a direct child component but instead requires a function as its child.
- This function automatically **receives the context value as a parameter**.
- It then returns the JSX that should be rendered using that context value.

Here’s our `Cart.jsx` component rewritten using `.Consumer`:

```jsx
import { CartContext } from "../store/shopping-cart-context.jsx";

export default function Cart({ onUpdateItemQuantity }) {
  return (
    <CartContext.Consumer>
      {(cartCtx) => {
        const totalPrice = cartCtx.items.reduce(
          (acc, item) => acc + item.price * item.quantity,
          0
        );
        const formattedTotalPrice = `$${totalPrice.toFixed(2)}`;

        return (
          <div id="cart">
            {cartCtx.items.length === 0 && <p>No items in cart!</p>}
            {cartCtx.items.length > 0 && (
              <ul id="cart-items">
                {cartCtx.items.map((item) => {
                  const formattedPrice = `$${item.price.toFixed(2)}`;
                  return (
                    <li key={item.id}>
                      <div>
                        <span>{item.name}</span>
                        <span> ({formattedPrice})</span>
                      </div>
                      <div className="cart-item-actions">
                        <button
                          onClick={() => onUpdateItemQuantity(item.id, -1)}
                        >
                          -
                        </button>
                        <span>{item.quantity}</span>
                        <button
                          onClick={() => onUpdateItemQuantity(item.id, 1)}
                        >
                          +
                        </button>
                      </div>
                    </li>
                  );
                })}
              </ul>
            )}
            <p id="cart-total-price">
              Cart Total: <strong>{formattedTotalPrice}</strong>
            </p>
          </div>
        );
      }}
    </CartContext.Consumer>
  );
}
```

### Comparing `useContext` vs. `.Consumer`
| Feature | `useContext` | `.Consumer` |
|---------|-------------|-------------|
| Readability | Easier and cleaner | More verbose and harder to read |
| Performance | Efficient | Less efficient due to function execution every render |
| Ease of Use | Directly extracts context value | Requires function as a child |
| Older Codebases | Newer standard | Common in older codebases |

### Why `useContext` is Preferred
While the `.Consumer` component provides an alternative, it is generally considered **less readable and more cumbersome** to use than `useContext`. The `useContext` hook allows for **cleaner, more intuitive** code by directly accessing context values without needing a function inside JSX.

### When to Use `.Consumer`
- If you are **working with an older React codebase** that still follows the `.Consumer` pattern.
- If you need **fine-grained control over rendering behavior** (though React.memo and other optimizations are generally better for performance concerns).
- If you **cannot use hooks** (e.g., in a class component). In class components, hooks like `useContext` are not available, making `.Consumer` necessary.

## Conclusion
This was just an **alternative** approach to consuming context in React, and we **will not be using it** going forward. Instead, we will stick with `useContext` due to its **better readability and maintainability**.

However, knowing about `.Consumer` is valuable because you may encounter it in legacy codebases or when working with class components. By understanding both methods, you can confidently navigate any React codebase, whether modern or older.

### Key Takeaways:
1. `useContext` is the modern and preferred way of consuming context in React function components.
2. `.Consumer` is an older approach that works in function and class components but is more verbose.
3. `.Consumer` requires a function as its child, which receives the context value and returns JSX.
4. `.Consumer` may still be relevant when working with older codebases or class components.
5. We are **sticking with `useContext`** for our project, but this knowledge helps us understand different approaches to React Context!

---
This expanded explanation should provide clarity and depth to your understanding of React Context consumption methods.

