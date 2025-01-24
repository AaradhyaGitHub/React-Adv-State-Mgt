# Context API Guide

## Component Composition

Component composition is a fundamental concept in React that helps manage and structure UI more effectively. While it can be a part of a solution to reduce prop drilling, it is not always the ultimate solution. We explore its use before transitioning to Context API.

## Refactoring `Shop.jsx`

We begin with the `Shop.jsx` component:

```javascript
import { DUMMY_PRODUCTS } from '../dummy-products.js';
import Product from './Product.jsx';

export default function Shop({ onAddItemToCart }) {
  return (
    <section id="shop">
      <h2>Elegant Clothing For Everyone</h2>
      <ul id="products">
        {DUMMY_PRODUCTS.map((product) => (
          <li key={product.id}>
            <Product {...product} onAddToCart={onAddItemToCart} />
          </li>
        ))}
      </ul>
    </section>
  );
}
```

## Moving Product List to `App.jsx`

To simplify `Shop`, we move the list of products to `App.jsx`.

### Advantages:
- The `onAddItemToCart` prop no longer needs to be passed down to `Shop`, reducing prop drilling.
- `Shop.jsx` becomes more reusable, focusing only on layout.

## Creating a Wrapper Around the Product List

To make `Shop.jsx` more flexible, we modify it to accept children:

```javascript
export default function Shop({ children }) {
  return (
    <section id="shop">
      <h2>Elegant Clothing For Everyone</h2>
      <ul id="products">{children}</ul>
    </section>
  );
}
```

## Updating `App.jsx` to Minimize Prop Drilling

In `App.jsx`, we now wrap the product list inside `Shop`:

```javascript
<Shop>
  {DUMMY_PRODUCTS.map((product) => (
    <li key={product.id}>
      <Product {...product} onAddToCart={handleAddItemToCart} />
    </li>
  ))}
</Shop>
```

### Observations

#### Reduced Prop Drilling:
- The `onAddItemToCart` prop is no longer passed through `Shop.jsx`.

#### Potential Downsides:
- Too many components may end up inside `App.jsx`, making it cluttered.
- Other components (like `Shop.jsx`) become simple wrapper components, reducing their role.
- `App.jsx` could become bloated if too much logic is centralized.

## Next Steps: Introducing Context API

While component composition helped reduce prop drilling, it is not a complete solution. For better state management and scalability, we introduce the Context API to manage global state efficiently without excessive prop drilling.

