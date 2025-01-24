# 🔗 Linking Context to State in React

## 🏗 The Static Nature of Our Context

Currently, our context value is **static**—it is always an empty array. However, we need to **link this to a state** that we are managing in `App.jsx`.

## 📌 Reminder: Our State in `App.jsx`

```jsx
function App() {
  const [shoppingCart, setShoppingCart] = useState({
    items: []
  });
```

Conveniently, our state follows the **exact same structure** as our context.

## 📌 Reminder: Our Context Definition

```jsx
import { createContext } from "react";
export const CartContext = createContext({
  items: []
});
```

At this point, our **context is static**, but we want it to be dynamic like `shoppingCart.items`.

## 🔗 Linking State to Context

We can simply do this:

```jsx
<CartContext.Provider value={shoppingCart}>
```

instead of:

```jsx
<CartContext.Provider value={{ items: [] }}>
```

✅ This makes our context dynamic and updates when the state updates!

## ⚠️ The Problem: State Updates via Context

While this allows us to **read** the context, **editing the state does not work** yet. We are still **passing props** to update the state:

```jsx
<Product {...product} onAddToCart={handleAddItemToCart} />
```

Ideally, we want to **handle everything through Context**—both reading and updating values—so we don't have to pass props.

## 🛠 Adding State-Updating Function to Context

We go back to **manually creating a context value**:

```jsx
const ctxValue = {
    items: shoppingCart.items,
    addItemToCart: handleAddItemToCart
};
```

- `items: shoppingCart.items` → Shares the state’s items array
- `addItemToCart: handleAddItemToCart` → Shares a function to add items

Now, **any component wrapped by the provider** can:
- **Read** the `items` array
- **Call** `addItemToCart()` to modify the state

## 🚀 Updating Context Provider

```jsx
<CartContext.Provider value={ctxValue}>
```

Now, **components can use our context!**

## 📌 Applying Context in `Product.jsx`

### ✅ Reminder: Current `Product` Component

```jsx
export default function Product({
  id,
  image,
  title,
  price,
  description,
  onAddToCart,
}) {
  return (
    <article className="product">
      <img src={image} alt={title} />
      <div className="product-content">
        <div>
          <h3>{title}</h3>
          <p className='product-price'>${price}</p>
          <p>{description}</p>
        </div>
        <p className='product-actions'>
          <button onClick={() => onAddToCart(id)}>Add to Cart</button>
        </p>
      </div>
    </article>
  );
}
```

### ❌ Removing `onAddToCart` Prop

We will replace `onAddToCart` with **context**.

```jsx
import { useContext } from "react";
import { CartContext } from "../store/shopping-cart-context.jsx";

const cartCtx = useContext(CartContext);
```

### 🧐 Why No Auto-Completion for `addItemToCart`?

As we try destructuring:

```jsx
const { items, addItemToCart } = useContext(CartContext);
```

We notice that `addItemToCart` **does not autocomplete**.

### 🔍 Fix: Include Function in Initial Context Value

We need to **update the initial context value**:

```jsx
import { createContext } from "react";
export const CartContext = createContext({
  items: [],
  addItemToCart: () => {},
});
```

### 🎯 Now, Update `Product.jsx`

```jsx
const { items, addItemToCart } = useContext(CartContext);
```

### ✅ Implementing the Context-Based Function

```jsx
<button onClick={() => addItemToCart(id)}>Add to Cart</button>
```

## 🎉 Everything Works!

- Context **provides values that can be read**
- Context **also provides functions that can be called** to update state
- State is **linked to context**, so changes reflect dynamically

## 🔥 Key Takeaways

- We **linked state to context** to make it dynamic.
- We **included a function in the context** to allow state updates.
- Components **now access state and update functions through context** instead of props.
- **No more prop drilling!**

### 🚀 Next Steps
- Implement this approach in all **cart-related components**.
- Remove **all cart-related props** from components.
- Ensure **state updates propagate correctly through context.**

