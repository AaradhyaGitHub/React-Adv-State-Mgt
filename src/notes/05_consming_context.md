# 📦 Consuming Context in the Cart Component

Now that the content has been provided, let's **consume** it!

## 🛒 Consuming the `items` in the `Cart` Component

We could consume the `items` array inside the `Cart` component. But **why?**

> We need `items` here to render them inside the cart, so we are passing them as **props**.

### ✨ Initial Code (Using Props)

```jsx
export default function Cart({ items, onUpdateItemQuantity }) {
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

---

## 🛠 Using Context Instead of Props

Now that we have **created a Context** with an `items` array value, we should **consume** that value instead of passing props manually.

### 🔌 Step 1: Importing the Context

Inside `Cart.jsx`, import the context:

```jsx
import { CartContext } from '../store/shopping-cart-context.jsx';
```

### 🔄 Step 2: Using a Hook to Access Context

To access the context, we use one of two hooks:

#### 🌀 `useContext` (Standard Approach)

- Call `useContext` **inside the component function** and pass the `CartContext` as an argument.
- Store the returned value in a **constant**.

```jsx
const cartCtx = useContext(CartContext);
```

#### 🔥 `use` Hook (Newer Approach in React 19+)

- Works similarly to `useContext`, but is **more flexible** as it can be used inside `if` blocks.
- Example:

```jsx
if (true) {
  const cartCtx = use(CartContext);
}
```

👉 We'll use `useContext` for now because `use` is only available in **React 19 and above**.

### 📝 Reminder: What Our Context Looks Like

```jsx
import { createContext } from "react";
export const CartContext = createContext({
  items: []
});
```

### ✨ Updated Code (Using Context)

Since we are now using `cartCtx.items`, we can **remove the `items` prop**:

#### 🚀 **Before** (Using Props)

```jsx
const totalPrice = items.reduce(
  (acc, item) => acc + item.price * item.quantity,
  0
);
const formattedTotalPrice = `$${totalPrice.toFixed(2)}`;
```

#### ⚡ **After** (Using Context)

```jsx
const totalPrice = cartCtx.items.reduce(
  (acc, item) => acc + item.price * item.quantity,
  0
);
const formattedTotalPrice = `$${totalPrice.toFixed(2)}`;
```

#### 🛠 Updated Component

```jsx
export default function Cart({ onUpdateItemQuantity }) {
  const cartCtx = useContext(CartContext);
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

---

## 🚨 Fixing the `Context.Provider` Error

When we run this, we get an error:

```
hook.js:608 The value prop is required for the <Context.Provider>
```

### Why Does This Happen? 🤔

Even though we set a **default value** in `shopping-cart-context.jsx`, it is **only used if a component is NOT wrapped inside a Provider**.

### 🔧 Fixing It in `App.jsx`

We need to **explicitly** provide a `value` inside our `CartContext.Provider`:

```jsx
<CartContext.Provider value={{ items: [] }}>
  <Cart />
</CartContext.Provider>
```

### ✅ Now, It Works!

Running the app again, we see:

```jsx
{cartCtx.items.length === 0 && <p>No items in cart!</p>}
```

Because by design, our context **starts as an empty array**! 🎉

---

## 🛠 Why Declare Context Separately?

Even though it might seem **repetitive** to declare the context in a separate file and again in the component:

✅ **Better Auto-Completion** – IDEs provide better hints when accessing `cartCtx`.

✅ **Scalability** – Makes it easier to manage context when the app grows.

✅ **Code Organization** – Keeps our app modular and maintainable.

🚀 Now, the `Cart` component dynamically consumes `items` from the **centralized shopping cart context**! 🎯

