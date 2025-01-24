# 🔍 Migrating to React Context: State Management

## 🔄 App.jsx Transformation

### Before Context Migration

```jsx
return (
  <CartContext value={ctxValue}>
    <Header
      cart={shoppingCart}
      onUpdateCartItemQuantity={handleUpdateCartItemQuantity}
    />
    <Shop>
      {DUMMY_PRODUCTS.map((product) => (
        <li key={product.id}>
          <Product {...product} onAddToCart={handleAddItemToCart} />
        </li>
      ))}
    </Shop>
  </CartContext>
);
```

### After Context Migration

```jsx
return (
  <CartContext value={ctxValue}>
    <Header />
    <Shop>
      {DUMMY_PRODUCTS.map((product) => (
        <li key={product.id}>
          <Product {...product} />
        </li>
      ))}
    </Shop>
  </CartContext>
);
```

## 🧩 Header Component Refactoring

### Initial Header Implementation

```jsx
import { useRef } from "react";

import CartModal from "./CartModal.jsx";

export default function Header({ cart, onUpdateCartItemQuantity }) {
  const modal = useRef();

  const cartQuantity = cart.items.length;

  function handleOpenCartClick() {
    modal.current.open();
  }

  let modalActions = <button>Close</button>;

  if (cartQuantity > 0) {
    modalActions = (
      <>
        <button>Close</button>
        <button>Checkout</button>
      </>
    );
  }

  return (
    <>
      <CartModal
        ref={modal}
        cartItems={cart.items}
        onUpdateCartItemQuantity={onUpdateCartItemQuantity}
        title="Your Cart"
        actions={modalActions}
      />
      <header id="main-header">
        <div id="main-title">
          <img src="logo.png" alt="Elegant model" />
          <h1>Elegant Context</h1>
        </div>
        <p>
          <button onClick={handleOpenCartClick}>Cart ({cartQuantity})</button>
        </p>
      </header>
    </>
  );
}
```

### Context-Based Quantity Retrieval

```jsx
import { useRef } from "react";
import { useContext } from "react";
const { items } = useContext(CartContext);
const cartQuantity = items.length;
```

### Simplified Header Component

```jsx
return (
  <>
    <CartModal ref={modal} title="Your Cart" actions={modalActions} />
    <header id="main-header">
      <div id="main-title">
        <img src="logo.png" alt="Elegant model" />
        <h1>Elegant Context</h1>
      </div>
      <p>
        <button onClick={handleOpenCartClick}>Cart ({cartQuantity})</button>
      </p>
    </header>
  </>
);
```

## 🌐 Key Context Migration Insights

- Remove prop passing for cart-related data
- Utilize `useContext` for state retrieval
- Simplify component interfaces
- Centralize state management

> **💡 Note**: Context allows for cleaner, more modular component design by eliminating prop drilling

# 🔍 Migrating to React Context: State Management

## 🛠 Refactoring CartModal

### Original Implementation
```jsx
const CartModal = forwardRef(function Modal(
  { cartItems, onUpdateCartItemQuantity, title, actions },
  ref
) {
  const dialog = useRef();

  useImperativeHandle(ref, () => ({
    open: () => {
      dialog.current.showModal();
    }
  }));

  return createPortal(
    <dialog id="modal" ref={dialog}>
      <h2>{title}</h2>
      <Cart items={cartItems} onUpdateItemQuantity={onUpdateCartItemQuantity} />
      <form method="dialog" id="modal-actions">
        {actions}
      </form>
    </dialog>,
    document.getElementById("modal")
  );
});
```

### Context Configuration in App.jsx
```jsx
const ctxValue = {
  items: shoppingCart.items,
  addItemToCart: handleAddItemToCart,
  updateItemQuantity: handleUpdateCartItemQuantity
};

export const CartContext = createContext({
  items: [],
  addItemToCart: () => {},
  updateItemQuantity: () => {}
});
```

### Refactored CartModal
```jsx
const CartModal = forwardRef(function Modal({ title, actions }, ref) {
  const dialog = useRef();

  useImperativeHandle(ref, () => ({
    open: () => {
      dialog.current.showModal();
    }
  }));

  return createPortal(
    <dialog id="modal" ref={dialog}>
      <h2>{title}</h2>
      <Cart />
      <form method="dialog" id="modal-actions">
        {actions}
      </form>
    </dialog>,
    document.getElementById("modal")
  );
});
```

## 🧩 Cart Component Transformation

### Context-Driven Implementation
```jsx
export default function Cart() {
  const { items, updateItemQuantity } = useContext(CartContext);

  return (
    <div>
      {items.map((item) => (
        <div key={item.id} className="cart-item">
          <span>{item.name}</span>
          <div className="cart-item-actions">
            <button onClick={() => updateItemQuantity(item.id, -1)}>-</button>
            <span>{item.quantity}</span>
            <button onClick={() => updateItemQuantity(item.id, 1)}>+</button>
          </div>
        </div>
      ))}
    </div>
  );
}
```

## ✂️ Prop Drilling Elimination

### Updated App.jsx
```jsx
return (
  <CartContext.Provider value={ctxValue}>
    <Header />
    <Shop>
      {DUMMY_PRODUCTS.map((product) => (
        <li key={product.id}>
          <Product {...product} />
        </li>
      ))}
    </Shop>
  </CartContext.Provider>
);
```

### Product Component
```jsx
export default function Product({ id, name, price }) {
  const { addItemToCart } = useContext(CartContext);

  return (
    <div className="product">
      <h3>{name}</h3>
      <p>Price: ${price}</p>
      <button onClick={() => addItemToCart({ id, name, price, quantity: 1 })}>
        Add to Cart
      </button>
    </div>
  );
}
```

## ✅ Refactoring Benefits
- 🚫 Eliminated prop drilling
- 🔄 Centralized state management
- 🧹 Simplified component interfaces
- 🎯 Focused component responsibilities

> **💡 Key Insight**: Context enables seamless state sharing without complex prop passing