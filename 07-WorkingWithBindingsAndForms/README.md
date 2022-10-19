### Two-Way Binding Refresher

```
    <script>
      let val = "Max"

      $: console.log(val)

      function setValue(event) {
        val = event.target.value
      }
    </script>

    <!-- <input type="text" value={val} on:input={setValue} /> -->
    <input type="text" bind:value={val} />

```

### Understanding Custom Component Bindings

Toogle.svelte:

```
    <script>
      export let chosenOption = 1
    </script>

    <button disabled={chosenOption === 1} on:click={() => (chosenOption = 1)}>Option 1</button>
    <button disabled={chosenOption === 2} on:click={() => (chosenOption = 2)}>Option 2</button>

```

CustomInput.svelte:

```
    <script>
      export let val
    </script>

    <input type="text" bind:value={val} />

```

App.svelte:

```
    <script>
      import CustomInput from "./CustomInput.svelte"
      import Toggle from "./Toggle.svelte"

      let val = "Max"
      let selectedOption = 1

      $: console.log(val)
      $: console.log(selectedOption)

      function setValue(event) {
        val = event.target.value
      }
    </script>

    <!-- <input type="text" value={val} on:input={setValue} /> -->
    <!-- <input type="text" bind:value={val} /> -->
    <CustomInput type="text" bind:val />

    <Toggle bind:chosenOption={selectedOption} />
```

### Relying on Automatic Number Conversion

```
    <script>
        let price = 0

        $: console.log(price)
      </script>

      <input type="number" bind:value={price} />
```

### Binding Checkboxes and Radio Buttons

```
    <script>
      let agreed
      let favColor = "red"

      $: console.log(agreed)
      $: console.log(favColor)
    </script>

    <label>
      <input type="checkbox" bind:checked={agreed} />
      Agree to terms?
    </label>

    <h1>Favorite Color?</h1>

    <!-- Radio Buttons -->
    <label>
      <input type="radio" name="color" value="red" bind:group={favColor} />
      Red
    </label>
    <label>
      <input type="radio" name="color" value="green" bind:group={favColor} />
      Green
    </label>
    <label>
      <input type="radio" name="color" value="blue" bind:group={favColor} />
      Blue
    </label>

    <!-- Checkboxes -->
    <label>
      <input type="checkbox" name="color" value="red" bind:group={favColor} />
      Red
    </label>
    <label>
      <input type="checkbox" name="color" value="green" bind:group={favColor} />
      Green
    </label>
    <label>
      <input type="checkbox" name="color" value="blue" bind:group={favColor} />
      Blue
    </label>
```

### Binding select Dropdowns

```
    <script>
      let favColor = ["green"]
      let singleFavColor = "red"

      $: console.log(favColor)
      $: console.log(singleFavColor)
    </script>

    <select bind:value={singleFavColor}>
      <option value="green">Green</option>
      <option value="red">Red</option>
      <option value="blue">Blue</option>
    </select>
```

### Binding to Element References

```
    <script>
      let usernameInput
      let someDiv

      function saveData() {
        // console.log(document.querySelector("#username").value)
        console.log(usernameInput.value)
        console.dir(usernameInput)
        console.dir(someDiv)
      }
    </script>

    <input type="text" bind:this={usernameInput} />
    <button on:click={saveData}>Save</button>

    <div bind:this={someDiv} />
```

### Binding to Component References

CustomInput.svelte:

```
    <script>
      export let val

      export function empty() {
        val = ""
      }
    </script>

    <input type="text" bind:value={val} />
```

App.svelte:

```
    <script>
      import CustomInput from "./CustomInput.svelte"

      let val = "Max"
      let customInput

      $: console.log(val)
      $: console.log(customInput)

      function setValue(event) {
        val = event.target.value
      }

      function saveData() {
        // console.log(document.querySelector("#username").value)
        console.log(usernameInput.value)
        console.dir(usernameInput)
        console.dir(someDiv)
        customInput.empty()
      }
    </script>

    <!-- <input type="text" value={val} on:input={setValue} /> -->
    <!-- <input type="text" bind:value={val} /> -->
    <CustomInput bind:val bind:this={customInput} />
```

### Validating Forms and Inputs

validation.js:

```
    export function isValidEmail(val) {
      return val.includes("@")
    }
```

App.svelte:

```
    <script>
      import { isValidEmail } from "./validation"

      let enteredEmail = ""
      let formIsValid = false

      $: if (isValidEmail(enteredEmail)) {
        formIsValid = true
      } else {
        formIsValid = false
      }
    </script>

    <form on:submit|preventDefault>
      <input type="email" bind:value={enteredEmail} class={isValidEmail(enteredEmail) ? "" : "invalid"} />
      <button type="submit" disabled={!formIsValid}>Save</button>
    </form>

    <style>
      .invalid {
        border: 1px solid red;
      }
    </style>
```
