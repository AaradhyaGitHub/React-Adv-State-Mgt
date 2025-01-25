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

    ```

- we can call this dispatch function and we use it to dispatch an action.
- This action can be anything: string, number
- in most cases, it's a object with a property of type or identifier so that we can set apart the actions from each other and handle them differently

```jsx
shoppingCartDispatch({
  type: "ADD_ITEM"
});
```
