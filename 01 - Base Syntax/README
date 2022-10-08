### Required Features
![Required Features](../images/Required%20Features.PNG)

### Using Curly Braces
Svelte will automatically look for a value that it can output here.
<pre>
    <code>
        <h1>Hello {name}, my age is {age}!</h1>
        <button on:click="{incrementAge}">Change Age</button>
    </code>
</pre>
- Using it like {name} and {age}, we should always refer to a variable or something related that can be as text
- Using it as a function, it does not execute the function automatically for you but under button `on:click` (syntax understood by Svelte - add an event listener to button) triggers and execute the function

### Reactive variables - Label statements ($:)
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/label
<pre>
    <code>
        `$: uppercaseName = name.toUpperCase();`
    </code>
</pre>
- It is an instruction picked up by the Svelte compiler. It changes/runs this code again because we use name on the right side of that labeled expression and therefore whenever name changes, it re-runs expression and stores the new value in uppercase name.
- Using this labeled statement, this dyanmic recalculation offered by Svelte is especially useful if you have multiple places in your page where you use that dynamic value because you do not have to use that in template markup manipulation everywhere

<pre>
    <code>
        `$: if (name === 'Maximilian') {
            console.log('It runs!');
            age = 31;
        }`
    </code>
</pre>
- It only console.log once because we are only referring to the dependencies specified in that line

### Binding to Element Properties
<pre>
    <code>
        <input type="text" value="{name}" on:input="{nameInput}" />
    </code>
</pre>
- If we want to change name through an input field, `on:input` listener input is a normal event which is fired by input elements whenever user types into that input, then, bind the input to the function
- `value:"{name}"` is used to let data flow into the input (uni directional dataflow)
- `on:input` is used to let data flow out of the input 
- With both `value:"{name}"` and `on:input`, it creates two directional data flow to reflect the input in input element and output

### Two-Way Binding shortcut
<pre>
    <code>
        <input type="text" bind:value="{name}" />
    </code>
</pre>
- `bind:value="{name}"` sets up two way binding where now data flows into value property but with every keystroke, it will also automatically update name 

### Using Multiple Compoenents
Svelte apps are built from multiple "Components" - UI building blocks. It is the same philosophy as in React, Angular and Vue.

<pre>
    <code>
        <ContactCard />
    </code>
</pre>
- root component: App.svelte (holds my entire Svelte application)
- subcomponents: ContactCard.svelte (import into App.svelte)

### Components & Components via Props
<pre>
    <code>
        // under script
        `export let userName;`

        <ContactCard userName="{name}" />
    </code>
</pre>

- Under script,  `export let` to add a variable can be set from outside
- We pass in variable as props in our subcomponent ContactCard

### Diving Deeper into Bindings
ContactCard.svelte:
<pre>
    <code>
        // under script
        `
        export let userName;
        export let jobTitle;
        export let description;
        export let userImage;
        `

        <div class="contact-card">
            <header>
                <div class="thumb">
                    <img src="{userImage}" alt="{userName}" />
                </div>
                <div class="user-data">
                    <h1>{userName}</h1>
                    <h2>{jobTitle}</h2>
                </div>
            </header>
            <div class="description">
                <p>{description}</p>
            </div>
        </div>
    </code>
</pre>

App.svelte:
<pre>
    <code>
        // under script
    	`
        import ContactCard from "./ContactCard.svelte";

        let name = "Max";
        let title = "";
        let image = "";
        let description = "";
        `

        <ContactCard 
            userName={name}
            jobTitle={title} 
            description={description} 
            userImage={image} />
    </code>
</pre>

### Using Self-Extending Properties
<pre>
    <code>
        <ContactCard 
            userName={name}
            jobTitle={title} 
            {description} 
            userImage={image} />
    </code>
</pre>
- If we have a property or an attribute which has the same name as the value `description={description}`, then Svelte allows us to use a shortcut `{description}` (curly braces)

### Outputting HTML content
<pre>
    <code>
        <div class="description">
            <p>{@html description}</p>
        </div>
    </code>
</pre>
- `@html description` tells Svelte that the result of this statement, the value of description variable should not be output as text but should be inserted as HTML into the DOM
- (not recommended) encourages Cross-Site Scripting attacks (XSS)

### Setting Dynamic CSS classes
<pre>
    <code>
        <div class="{userImage ? 'thumb' : 'thumb thumb-placeholder'}">
            <img src="{userImage}" alt={userName}/>
        </div>
    </code>
</pre>
<pre>
    <code>
        <div class="thumb" class:thumb-placeholder="{!userImage}">
            <img src="{userImage}" alt={userName}/>
        </div>
    </code>
</pre>