# Overview of the Context API Project

## App Structure & Major Components
This React project is structured to manage a shopping cart, with plans to integrate Context API for state management. Below is a breakdown of the major moving parts.

### **1. App.jsx (Root Component)**
- Manages the **shopping cart state** using `useState`.
- Defines `handleAddItemToCart` and `handleUpdateCartItemQuantity` to update the cart.
- Passes cart data and functions to **Header** and **Shop** components.

### **2. Shop.jsx (Product Listing Page)**
- Displays a list of products using `DUMMY_PRODUCTS`.
- Uses the `Product` component for each item.
- Calls `onAddItemToCart` when a product is added to the cart.

### **3. Product.jsx (Single Product Display)**
- Renders product details (image, title, price, description).
- Has an "Add to Cart" button that triggers `onAddToCart`.

### **4. Header.jsx (Navigation & Cart Button)**
- Displays a **Cart button** showing the number of items.
- Uses `CartModal` to show the cart when the button is clicked.
- Calls `handleOpenCartClick` to open the modal.

### **5. CartModal.jsx (Cart Popup Using Portals)**
- Uses `forwardRef` and `useImperativeHandle` to control visibility.
- Uses `createPortal` to render in a separate DOM node (`#modal`).
- Displays the `Cart` component inside the modal.

### **6. Cart.jsx (Cart Item List & Total Price)**
- Displays items in the cart with quantity controls (`+` and `-` buttons).
- Calls `onUpdateItemQuantity` to update or remove items.
- Computes and displays the total cart price.

## **State Management (Before Context API)**
- The `App.jsx` component maintains cart state and passes it down as props.
- `handleAddItemToCart` and `handleUpdateCartItemQuantity` update cart items.
- Components like `Header`, `Shop`, and `Cart` receive cart-related data via props.

## **Why Context API Will Be Useful?**
- Currently, prop drilling is happening (passing state multiple levels deep).
- Context API will centralize state, making it accessible without excessive prop drilling.
- Components like `CartModal`, `Cart`, and `Product` will benefit from direct access to cart data.

### **Next Steps**
The next step would be to refactor the cart state using Context API, likely creating a `CartContext` to manage cart-related functions globally.

