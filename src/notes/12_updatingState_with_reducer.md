# 🛒 React Shopping Cart State Management Tutorial

## 🔍 Reducer Initial State

### Starting Point

```jsx
function shoppingCartReducer() {
  return state;
}
```

## 🚀 Action Dispatching Journey

### Dispatch Concept Visualization

```
   Action Object
   ┌───────────────┐
   │ type: "ADD_ITEM"│
   │ payload: id    │
   └───────────────┘
          ↓
   [Dispatch Function]
```

### Dispatch Examples

```jsx
// Basic Dispatch
shoppingCartDispatch({
  type: "ADD_ITEM"
});

// Enhanced Dispatch with Payload
shoppingCartDispatch({
  type: "ADD_ITEM",
  payload: id
});
```

## 🧩 State Update Logic

### Original State Update Method

```jsx
function handleAddItemToCart(id) {
  setShoppingCart((prevShoppingCart) => {
    // Complex state update logic here
    // (full original code preserved)
  });
}
```

### Reducer Transformation Flow

```
[Original State Update]
        ↓
[Move Logic to Reducer]
        ↓
[Dispatch-Driven Updates]
```

### Reducer Implementation Stages

1. 📌 Identify Action Type
2. 🔄 Create Updated Items
3. 🆕 Return New State

## 💡 Key Reducer Patterns

### Basic State Return

```jsx
return {
  items: updatedItems
};
```

### Complex State Preservation

```jsx
return {
  ...state, // Preserve other state properties
  items: updatedItems
};
```

## 🔑 Critical Insights

- Actions communicate state changes
- Reducers centralize update logic
- Immutability is key
- Payload carries necessary data

---

---

Now to actually edit the state, we need to edit our reducer function i.e this one:

```jsx
function shoppingCartReducer() {
  return state;
}
```

We need to handle different actions that will lead to a state update:

Let's start by dispatching a action with help of that shoppingCartDispatch

The idea:

- handleAddItemToCart here, insted of having all the state updating logic here, like so:

  ```jsx

  function handleAddItemToCart(id) {

  setShoppingCart((prevShoppingCart) => {
    const updatedItems = [...prevShoppingCart.items];

    const existingCartItemIndex = updatedItems.findIndex(
      (cartItem) => cartItem.id === id
    );
    const existingCartItem = updatedItems[existingCartItemIndex];

    if (existingCartItem) {
      const updatedItem = {
        ...existingCartItem,
        quantity: existingCartItem.quantity + 1
      };
      updatedItems[existingCartItemIndex] = updatedItem;
    } else {
      const product = DUMMY_PRODUCTS.find((product) => product.id === id);
      updatedItems.push({
        id: id,
        name: product.title,
        price: product.price,
        quantity: 1
      });
    }

    return {
      items: updatedItems
    };
  });
  ```

}

- we can call this dispatch function and we use it to dispatch an action.
- This action can be anything: string, number
- in most cases, it's a object with a property of type or identifier so that we can set apart the actions from each other and handle them differently

```jsx
shoppingCartDispatch({
  type: "ADD_ITEM"
});
```

- now, we can identify the action
- now, this action has some data attached to it required to perform the action, in this case, id of the product to add to the cart
- let's add a second property. It can be called anything but usually it's called payload
- in or case, the payload is a `id` set on the action object

```jsx
shoppingCartDispatch({
  type: "ADD_ITEM",
  payload: id
});
```

- Now, we can go back to the reducer function and whenevr the action is dispatched, the shoppingCartReducer function will be called
  and we'll get that action so the argument we passed as a value for the action parameter

```jsx
function shoppingCartReducer() {
  if (useActionState.type === "ADD_ITEM") {
    //...
  }
  return state;
}
```

- Now, we want to update the state based on the previous state, now we can copy the logic to the `shoppingCartReducer` function but we have to tweak it of course

- then we can get rid of the stateUpdating function i.e the setShoppingCart function.

LIke so:

```jsx
function shoppingCartReducer() {
  if (useActionState.type === "ADD_ITEM") {
    const updatedItems = [...prevShoppingCart.items];

    const existingCartItemIndex = updatedItems.findIndex(
      (cartItem) => cartItem.id === id
    );
    const existingCartItem = updatedItems[existingCartItemIndex];

    if (existingCartItem) {
      const updatedItem = {
        ...existingCartItem,
        quantity: existingCartItem.quantity + 1
      };
      updatedItems[existingCartItemIndex] = updatedItem;
    } else {
      const product = DUMMY_PRODUCTS.find((product) => product.id === id);
      updatedItems.push({
        id: id,
        name: product.title,
        price: product.price,
        quantity: 1
      });
    }

    return {
      items: updatedItems
    };
  }
  return state;
}
```

- Now, the prevShoppingCart value is now that `state` parameter
- the `id` can be retrieved from the action i.e `action.payload`
- then we're returning the new updated state as such

```jsx
return {
  items: updatedItems
};
```

- if we had a more complex state object with multiple properties, we might want to copy and spread the old items first so we don't lose anything and we just update the one value
- Like so:

```jsx
return {
  ...state,
  items: updatedItems
};
```

- With that, we are handling the `ADD_ITEM` action
- The code of course is not less than before but the logic is outside the Component and we don't have any special function form because we are automatically getting the state snapshot
- We can now migrate the rest
- Let's migrate `handleUpdateCartItemQuantity`
- It currently looks like this:

```jsx
function handleUpdateCartItemQuantity(productId, amount) {
  setShoppingCart((prevShoppingCart) => {});

  const updatedItems = [...prevShoppingCart.items];
  const updatedItemIndex = updatedItems.findIndex(
    (item) => item.id === productId
  );

  const updatedItem = {
    ...updatedItems[updatedItemIndex]
  };

  updatedItem.quantity += amount;

  if (updatedItem.quantity <= 0) {
    updatedItems.splice(updatedItemIndex, 1);
  } else {
    updatedItems[updatedItemIndex] = updatedItem;
  }

  return {
    items: updatedItems
  };
}
```
- Then we call the dispatch function istead:
```jsx
  function handleUpdateCartItemQuantity(productId, amount) {
    shoppingCartDispatch({
      type: 'UPDATE_ITEM',
      payload: {
        productId,
        amount
      }
    })
   
  }
```
- We also got rid of `setShoppingCart`
- Then all that is copied into a second if block 
like so:
```jsx
if(action.type === 'UPDATE_ITEM'){
    const updatedItems = [...state.items];
      const updatedItemIndex = updatedItems.findIndex(
        (item) => item.id === action.payload.productId
      );

      const updatedItem = {
        ...updatedItems[updatedItemIndex]
      };

      updatedItem.quantity += action.payload.amount;

      if (updatedItem.quantity <= 0) {
        updatedItems.splice(updatedItemIndex, 1);
      } else {
        updatedItems[updatedItemIndex] = updatedItem;
      }

      return {
        items: updatedItems
      };
  }
```
 
