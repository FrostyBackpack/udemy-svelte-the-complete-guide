### Updating Arrays and Objects Immutably

https://academind.com/tutorials/reference-vs-primitive-values

Update arrays by assigning a new array and not by using push/pop instead always store as new value in variables which you use in your markup. You should always reassign a value with a new = if you want Svelte to update the real dom as the = sign allows Svelte check what was changed and then whatever that is, output somewhere in the dom and whatever there should change.

Svelte will not do anything:
Array push will not do anything because there is no = sign involved

Svelte did something:
Array slice/filter using = sign

```
    // createdContacts.push({ name: name, jobTitle: title, imageUrl: image, desc: description })

    createdContacts = [...createdContacts, { id: Math.random(), name: name, jobTitle: title, imageUrl: image, desc: description }]
```

### Understanding Event Modifiers

https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Building_blocks/Events#event_bubbling_and_capture

5 supported modifiers:

- `once`: makes sure that event can only fire once and after the event, listeners simply is detached
- `passive`: improve scrolling performance (do not have to do it as Svelte will do it automatically in more cases)
- `capture`: fire event handler during the capture phase instead of public phase
- `stopPropagration`: stops event propagation
- `preventDefault`: prevent the default so that no request is sent (useful if you have a button inside a form)

```
    <button on:click|once={addContact}>Add Contact Card</button>
```

### Using Inline Functions

Calling event through function by providing a pointer so that when the event occurs, the function can be executed. This would be the recommended way of doing it because this is a really clear way of defining your functions up there in the script text, making it easy for other developers to understand your code.

If you have multiline statements, but the more logic you put in here, it is recommended to just point in function and logic up in the function script text.

Inline function (short and concise statement - fits into 1 line):

```
    <button on:click={(event) => {createContacts = createdContacts.slice(1);}}>Delete First</button>
```
