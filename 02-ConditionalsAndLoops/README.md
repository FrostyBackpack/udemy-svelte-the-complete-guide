### Using "if" statements in HTML

https://svelte.dev/tutorial/if-blocks

```
    {#if done}
    <ContactCard userName={name} jobTitle={title} {description} userImage={image} />
    {/if}
```

### if, else and else-if

Instead of using two "if" statements:

```
    {#if formState === "done"}
    <ContactCard userName={name} jobTitle={title} {description} userImage={image} />
    {/if}
    {#if formState === "invalid"}
      <p>Invalid input.</p>
    {/if}
```

Use intermediate "if" conditionals statement:

```
    {#if formState === "done"}
    <ContactCard userName={name} jobTitle={title} {description} userImage={image} />
    {:else if formState === "invalid"}
      <p>Invalid input.</p>
    {:else}
      <p>Please enter some data and hit the button!</p>
    {/if}
```

### Outputting Lists with "each"

https://svelte.dev/tutorial/each-blocks
https://academind.com/tutorials/reference-vs-primitive-values

Push items into array and display array items:

```
    let createdContacts = []
    createdContacts = [...createdContacts, { name: name, jobTitle: title, imageUrl: image, desc: description }]

    {#if formState === "invalid"}
      <p>Invalid input.</p>
    {:else}
      <p>Please enter some data and hit the button!</p>
    {/if}

    {#each createdContacts as contact}
      <ContactCard userName={contact.name} jobTitle={contact.jobTitle} description={contact.desc} userImage={contact.imageUrl} />
    {/each}
```

### "each", "else" and Extracing the Index

```
    {#each createdContacts as contact, index}
    <h2># {index + 1}</h2>
    <ContactCard userName={contact.name} jobTitle={contact.jobTitle} description={contact.desc} userImage={contact.imageUrl} />
    {:else}
      <p>Please start adding some contacts, we found none!</p>
    {/each}
```

- "else": when array is empty, this message will be displayed
- index: number of items in array

### Lists and Keys

https://svelte.dev/tutorial/keyed-each-blocks

If we do not have database, we have no unique ID for Svelte to know which selected item to be deleted as Svelte deletes item from the back of list. Unique id can be anything (numbers/strings/objects/arrays) but objects and arrays are reference types and to avoid nasty bugs, it is recommended that we use numbers/strings as id.

```
    function deleteFirst() {
      // slice is a built-in method in JavaScript
      // returns a new array and it starts at this position (index 1 - second element)
      // giving us the whole array starting at the next one (drops the first element)
      createdContacts = createdContacts.slice(1)
    }

    function deleteLast() {
      // slice is a built-in method in JavaScript
      // returns a new array and it starts at the first element
      // giving us the whole arrayup to minus one (until the element before the last)
      createdContacts = createdContacts.slice(0, -1)
    }

    {#each createdContacts as contact, index (contact.id)}
      <h2># {index + 1}</h2>
      <ContactCard userName={contact.name} jobTitle={contact.jobTitle} description={contact.desc} userImage={contact.imageUrl} />
    {:else}
      <p>Please start adding some contacts, we found none!</p>
    {/each}
```
