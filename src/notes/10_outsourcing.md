# Understanding Context in React: Managing Shopping Cart Data

## Problem

In a typical React application, especially when it grows in complexity, the `App` component can become very bloated. This happens because you may have to handle multiple contexts directly within the `App` component, leading to a large and hard-to-manage codebase. 

Instead of maintaining all the context management code inside the `App` component, it's beneficial to extract context logic into separate components. This makes the code cleaner, more modular, and easier to maintain.

## Solution: Using Context Separately

In this guide, we'll learn how to offload the context-related data management into a separate context provider component, which helps in maintaining a clean and scalable code structure.

### Step 1: Move Context Code Out of `App`

The initial context-related code managing the shopping cart state is defined in the `App` component. Let's look at that:

```jsx
const [shoppingCart, setShoppingCart] = useState({
    items: []
});

function handleAddItemToCart(id) {
    setShoppingCart((prevShoppingCart) => {
        const updatedItems = [...prevShoppingCart.items];
        // Add or update item logic
        return { items: updatedItems };
    });
}

function handleUpdateCartItemQuantity(productId, amount) {
    setShoppingCart((prevShoppingCart) => {
        const updatedItems = [...prevShoppingCart.items];
        // Update item quantity or remove item logic
        return { items: updatedItems };
    });
}

const ctxValue = {
    items: shoppingCart.items,
    addItemToCart: handleAddItemToCart,
    updateItemQuantity: handleUpdateCartItemQuantity,
};
```

This is moved into a new context provider component so that the App can simply consume the context rather than managing it.

### Step 2: Create a New Context Provider Component

We'll now create a CartContext and a CartContextProvider component to handle the cart logic. This separates the concerns of managing the shopping cart data.

```jsx
import { createContext, useState } from "react";
import { DUMMY_PRODUCTS } from "../dummy-products.js";
import Cart from "../components/Cart.jsx";

// Creating the CartContext
export const CartContext = createContext({
  items: [],
  addItemToCart: () => {},
  updateItemQuantity: () => {}
});

export default function CartContextProvider({children}) {
    const [shoppingCart, setShoppingCart] = useState({
        items: []
    });

    function handleAddItemToCart(id) {
        setShoppingCart((prevShoppingCart) => {
            const updatedItems = [...prevShoppingCart.items];
            // Add or update item logic
            return { items: updatedItems };
        });
    }

    function handleUpdateCartItemQuantity(productId, amount) {
        setShoppingCart((prevShoppingCart) => {
            const updatedItems = [...prevShoppingCart.items];
            // Update item quantity or remove item logic
            return { items: updatedItems };
        });
    }

    const ctxValue = {
        items: shoppingCart.items,
        addItemToCart: handleAddItemToCart,
        updateItemQuantity: handleUpdateCartItemQuantity
    };

    return (
        <CartContext.Provider value={ctxValue}>
            {children}    
        </CartContext.Provider>
    );
}
```

### Step 3: Using the Context in the App Component

Now, in your App component, you can simply wrap the components that need to access the shopping cart context with CartContextProvider. This keeps App clean and free from context management logic.

Initially, this is our App.jsx:

```jsx
import { useState } from "react";

import Header from "./components/Header.jsx";
import Shop from "./components/Shop.jsx";
import { DUMMY_PRODUCTS } from "./dummy-products.js";
import { CartContext } from "./store/shopping-cart-context.jsx";

import Product from "./components/Product.jsx";

function App() {

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
}

export default App;

```
First let's fix our CartContext import:
```jsx
import { CartContext } from "./store/shopping-cart-context.jsx";
```
To 
```jsx
import CartContextProvider from "./store/shopping-cart-context.jsx";

```


Then our App.jsx
```jsx
import { useState } from "react";

import Header from "./components/Header.jsx";
import Shop from "./components/Shop.jsx";
import { DUMMY_PRODUCTS } from "./dummy-products.js";
import CartContextProvider from "./store/shopping-cart-context.jsx";

import Product from "./components/Product.jsx";

function App() {
  return (
    <CartContextProvider>
      <Header />
      <Shop>
        {DUMMY_PRODUCTS.map((product) => (
          <li key={product.id}>
            <Product {...product} />
          </li>
        ))}
      </Shop>
    </CartContextProvider>
  );
}

export default App;

```

### Step 4: Consume the Context

Once you've wrapped your components with the CartContextProvider, you can consume the context in any child component using the useContext hook.

For example, in Cart.jsx:

```jsx
import { useContext } from "react";
import { CartContext } from "../context/CartContextProvider";

function Cart() {
    const { items, addItemToCart, updateItemQuantity } = useContext(CartContext);

    return (
        <div>
            {items.map(item => (
                <div key={item.id}>
                    {item.name} - {item.quantity}
                </div>
            ))}
        </div>
    );
}
```

## Key Concepts

- Context: Context is used to share state or logic globally within a component tree without passing props down manually through every level.
- Context Provider: A component that holds the context's value and provides it to its child components.
- useContext Hook: A hook used by child components to consume the values provided by a Context.Provider.
- Separation of Concerns: By moving the context logic to a separate CartContextProvider component, we ensure that the App component remains clean and focused on rendering, while the shopping cart logic is managed separately.

## Why This Is Beneficial

- Modularization: This approach modularizes your code, making it easier to manage and scale.
- Separation of Logic: It separates the UI rendering logic from the business logic, making your components simpler and more readable.
- Clean App Component: Your App component no longer has to handle context-related logic, reducing its complexity.

## Key Takeaways

1. Use context to manage state that needs to be shared across multiple components.
2. Extract context logic into separate context provider components to keep your `App` component clean.
3. Use `useContext` to consume context values in child components.
4. Wrapping your components with a context provider is a way to inject the context values into those components.

This pattern will help maintain clarity and structure in larger React applications.