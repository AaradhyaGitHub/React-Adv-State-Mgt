Component Composition 

Embrace component composition. This can be part of the overall situation. 
It can be a part of a solution
It's may not always be the full ultimate solution, always 

We could start with Shop.jsx:

```jsx
import { DUMMY_PRODUCTS } from '../dummy-products.js';
import Product from './Product.jsx';

export default function Shop({ onAddItemToCart }) {
  return (
    <section id="shop">
      <h2>Elegant Clothing For Everyone</h2>

      <ul id="products">
        
      </ul>
    </section>
  );
}
```
We can move the list of products from Shop to App...

Advantage, the pointer at App then, could be avoided:
      <Shop onAddItemToCart={handleAddItemToCart} />

That about there could be avoided. No need to pass it to Shop. 

We want to create a wrapper around the list of products. So we convert the Shop component:

```jsx 

export default function Shop({ children }) {
  return (
    <section id="shop">
      <h2>Elegant Clothing For Everyone</h2>
      <ul id="products">{children}</ul>
    </section>
  );
}
  


```

We modify the App return to minimize prop drilling:

```jsx

<Shop>
        {DUMMY_PRODUCTS.map((product) => (
          <li key={product.id}>
            <Product {...product} onAddToCart={handleAddItemToCart} />
          </li>
        ))}
</Shop>

```