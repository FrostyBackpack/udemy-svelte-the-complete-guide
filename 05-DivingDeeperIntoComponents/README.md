### Component Types

![Component Types](../images/Different%20Component%20Types.png)

### Nested Components and Communication

A typical application built with Svelte looks something like this.

- Can have multiple root components
- Typically, each root component has a tree of subcomponents, which the components you use by embedding them by their tags with your custom tags in the markup of your app component

![Nested Components and Communication](../images/Nested%20Components%20and%20Communication.png)

### Event Forwarding

Product.svelte:
```
        <article>
            <h1>{productTitle}</h1>
            <button on:click>Add to Cart</button>
            <button>Delete</button>
        </article>
```

App.svelte:
```
        <Product productTitle="A book" on:click={() => alert("Clicked!")} />
```

### Emitting Custom Events

Product.svelte:
```
        <script>
            import { createEventDispatcher } from "svelte"
            
            export let productTitle

            const dispatch = createEventDispatcher()

            function addToCart() {
                dispatch("add-to-cart", {id: "p1"})
            }
        </script>

        <article>
            <h1>{productTitle}</h1>
            <button on:click="{addToCart}">Add to Cart</button>
            <button on:click="{() => {dispatch("delete", "p1")}}">Delete</button>
        </article>
```

App.svelte:
```
        <Product productTitle="A book" on:add-to-cart={() => alert("Add to cart!")} on:delete={()  => alert("Delete!")} />
```

### How to Extract Event Data

App.svelte:

```
        <script>
            import Product from "./Product.svelte"

            function addToCart(event) {
                console.log(event.detail)
            }

            function deleteProduct(event) {
                console.log(event.detail)
            }
        </script>

        <Product productTitle="A book" on:add-to-cart={addToCart} on:delete={deleteProduct} />
```

### Using Spread Props and Default Props

Product.svelte:
```
        <script>
            import { createEventDispatcher } from "svelte"
            
            export let title
            export let price
            export let bestseller = false

            const dispatch = createEventDispatcher()

            function addToCart() {
                dispatch("add-to-cart", {id: "p1"})
            }
        </script>

        <article>
            <h1>{title}</h1>
            <h2>${price}</h2>
            {#if bestseller}
                <h3>BESTSELLER</h3>
            {/if}
            <button on:click="{addToCart}">Add to Cart</button>
            <button on:click="{() => {dispatch("delete", "p1")}}">Delete</button>
        </article>
```

App.svelte:
```
        <script>
            import Product from "./Product.svelte"

            let products = [
                {
                    id: "p1",
                    title: "A book",
                    price: 9.99
                }
            ]

            function addToCart(event) {
                console.log(event.detail)
            }

            function deleteProduct(event) {
                console.log(event.detail)
            }
        </script>

        {#each products as product}
            <Product {...product} on:add-to-cart={addToCart} on:delete={deleteProduct} />
            <!-- <Product title={product.title} price={product.price} on:add-to-cart={addToCart} on:delete={deleteProduct} /> -->
        {/each}
```

### Working with Slots
![Working with Slots](../images/Slot.PNG)

Modal.svelte:
```
        <div class="modal">
            <slot />
        </div>
```

App.svelte:
```
        <Modal>
            <h1>Hello!</h1>
            <p>This works!</p>
        </Modal>
```

### Named and Default Slots
