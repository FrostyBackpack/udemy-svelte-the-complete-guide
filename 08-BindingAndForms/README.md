### Exploring different validation solutions

```
    <script>
      let title = ""
      let titleValid = false
    </script>

    <TextInput
      id="title"
      label="Title"
      valid={titleValid}
      validityMessage="Please enter a valid title."
      value={title}
      on:input={event => (title = event.target.value)} />
```

### Adding Validation and Error Output to "TextInput" component

```
    <script>
      export let controlType = null;
      export let id;
      export let label;
      export let rows = null;
      export let value;
      export let type = "text";
      export let valid = true;
      export let validityMessage = "";
    </script>

    <style>
      input,
      textarea {
        display: block;
        width: 100%;
        font: inherit;
        border: none;
        border-bottom: 2px solid #ccc;
        border-radius: 3px 3px 0 0;
        background: white;
        padding: 0.15rem 0.25rem;
        transition: border-color 0.1s ease-out;
      }

      input:focus,
      textarea:focus {
        border-color: #e40763;
        outline: none;
      }

      label {
        display: block;
        margin-bottom: 0.5rem;
        width: 100%;
      }

      .form-control {
        padding: 0.5rem 0;
        width: 100%;
        margin: 0.25rem 0;
      }

      .invalid {
        border-color: red;
        background: #fde3e3;
      }

      .error-message {
        color: red;
        margin: 0.25rem 0;
      }
    </style>

    <div class="form-control">
      <label for={id}>{label}</label>
      {#if controlType === 'textarea'}
        <textarea class:invalid="{!valid}" {rows} {id} {value} on:input />
      {:else}
        <input class:invalid="{!valid}" {type} {id} {value} on:input />
      {/if}
      {#if validityMessage && !valid}
        <p class="error-message">{validityMessage}</p>
      {/if}
    </div>
```

### Adding some validation logic

Validation.js:

```
    export function isEmpty(val) {
      return val.trim().length === 0;
    }
```

EditMeetup.svelte:

```
    let title = "";
    let titleValid = false;
    let subtitle = "";
    let address = "";
    let email = "";
    let description = "";
    let imageUrl = "";

    const dispatch = createEventDispatcher();

    $: titleValid = !isEmpty(title);
```

### Finishing "TextInput" Validation

Validation.js:

```
    export function isEmpty(val) {
      return val.trim().length === 0
    }

    export function isValidEmail(val) {
      return new RegExp("[a-z0-9!#$%&'*+/=?^_`{|}~-]+(?:.[a-z0-9!#$%&'*+/=?^_`{|}~-]+)*@(?:[a-z0-9](?:[a-z0-9-]*[a-z0-9])?.)+[a-z0-9](?:[a-z0-9-]*[a-z0-9])?").test(val)
    }
```

EditMeetup.svelte:

```
    <script>
      import { createEventDispatcher } from "svelte"
      import TextInput from "../UI/TextInput.svelte"
      import Button from "../UI/Button.svelte"
      import Modal from "../UI/Modal.svelte"
      import { isEmpty } from "../helpers/validation.js"
      import { isValidEmail } from "../../../07-WorkingWithBindingsAndForms/src/validation"

      let title = ""
      let subtitle = ""
      let address = ""
      let email = ""
      let description = ""
      let imageUrl = ""
      let formIsValid = false

      const dispatch = createEventDispatcher()

      $: titleValid = !isEmpty(title)
      $: subtitleValid = !isEmpty(subtitle)
      $: addressValid = !isEmpty(address)
      $: descriptionValid = !isEmpty(description)
      $: imageUrlValid = !isEmpty(imageUrl)
      $: emailValid = isValidEmail(email)
      $: formIsValid = titleValid && subtitleValid && addressValid && descriptionValid && imageUrlValid && emailValid

      function submitForm() {
        dispatch("save", {
          title: title,
          subtitle: subtitle,
          address: address,
          email: email,
          description: description,
          imageUrl: imageUrl
        })
      }

      function cancel() {
        dispatch("cancel")
      }
    </script>

    <Modal title="Edit Meetup Data" on:cancel>
      <form on:submit|preventDefault={submitForm}>
        <TextInput id="title" label="Title" valid={titleValid} validityMessage="Please enter a valid title." value={title} on:input={event => (title = event.target.value)} />
        <TextInput id="subtitle" label="Subtitle" valid={subtitleValid} validityMessage="Please enter a valid subtitle." value={subtitle} on:input={event => (subtitle = event.target.value)} />
        <TextInput id="address" label="Address" valid={addressValid} validityMessage="Please enter a valid address." value={address} on:input={event => (address = event.target.value)} />
        <TextInput id="imageUrl" label="Image URL" valid={imageUrlValid} validityMessage="Please enter a valid image URL." value={imageUrl} on:input={event => (imageUrl = event.target.value)} />
        <TextInput id="email" label="E-Mail" type="email" valid={emailValid} validityMessage="Please enter a valid email address." value={email} on:input={event => (email = event.target.value)} />
        <TextInput
          id="description"
          label="Description"
          valid={descriptionValid}
          validityMessage="Please enter a valid description."
          controlType="textarea"
          bind:value={description}
          on:input={event => (description = event.target.value)}
        />
      </form>
      <div slot="footer">
        <Button type="button" mode="outline" on:click={cancel}>Cancel</Button>
        <Button type="button" on:click={submitForm} disabled={!formIsValid}>Save</Button>
      </div>
    </Modal>
```

TextInput.svelte:

```
    <script>
      export let controlType = null
      export let id
      export let label
      export let rows = null
      export let value
      export let type = "text"
      export let valid = true
      export let validityMessage = ""

      let touched = false
    </script>

    <div class="form-control">
      <label for={id}>{label}</label>
      {#if controlType === "textarea"}
        <textarea class:invalid={!valid && touched} {rows} {id} bind:value on:blur={() => (touched = true)} />
      {:else}
        <input class:invalid={!valid && touched} {type} {id} {value} on:input on:blur={() => (touched = true)} />
      {/if}
      {#if validityMessage && !valid && touched}
        <p class="error-message">{validityMessage}</p>
      {/if}
    </div>
```
