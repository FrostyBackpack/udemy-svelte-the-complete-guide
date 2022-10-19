### Adding Default Props

TextInput.svelte:

```
    <script>
      export let controlType = null
      export let id
      export let label
      export let rows = null
      export let value
      export let type = "text"
    </script>
```

App.svelte:

```
    <main>
      <!-- preventDefault: no HTTP requests get sent instead we handle this entirely on client side -->
      <!-- bind:value={variable}: set the initial value as empty string as value for input and once typed, the entered value will be saved back into variable -->
      <form on:submit|preventDefault={addMeetUp}>
        <TextInput id="title" label="Title" value={title} on:input={event => (title = event.target.value)} />

        <TextInput id="subtitle" label="Subtitle" value={subtitle} on:input={event => (subtitle = event.target.value)} />

        <TextInput id="address" label="Address" value={address} on:input={event => (address = event.target.value)} />

        <TextInput id="imageUrl" label="Image URL" value={imageUrl} on:input={event => (imageUrl = event.target.value)} />

        <TextInput id="email" label="E-mail" type="email" value={email} on:input={event => (email = event.target.value)} />

        <TextInput id="description" label="Description" controlType="textarea" value={description} on:input={event => (description = event.target.value)} />

        <Button type="submit" caption="Save" />
      </form>
      <MeetUpGrid {meetups} />
    </main>
```

### Communication via Custom Events

MeetUpItem.svelte:

```
    <article>
      <header>
        <h1>{title}</h1>
        <h2>{subtitle}</h2>
        <p>{address}</p>
      </header>
      <div class="image">
        <img src={imageUrl} alt={title} />
      </div>
      <div class="content">
        <p>{description}</p>
      </div>
      <footer>
        <Button href="mailto:{email}" caption="Contact" />
        <Button mode="outline" type="button" caption={isFav ? "Unfavorite" : "Favorite"} on:click={() => dispatch("togglefavorite", id)} />
        <Button type="button" caption="Show Details" />
      </footer>
    </article>
```

MeetUpGrid.svelte:

```
    <section id="meetups">
      {#each meetups as meetup}
        <MeetupItem
          id={meetup.id}
          title={meetup.title}
          subtitle={meetup.subtitle}
          description={meetup.description}
          imageUrl={meetup.imageUrl}
          email={meetup.contactEmail}
          address={meetup.address}
          isFav={meetup.isFavorite}
          on:togglefavorite
        />
      {/each}
    </section>
```

App.svelte:

```
    function toggleFavorite(event) {
        console.log(event)
        const id = event.detail
        // returns a single meetup object if true/found
        const updatedMeetup = { ...meetups.find(m => m.id === id) }
        updatedMeetup.isFavorite = !updatedMeetup.isFavorite
        const meetupIndex = meetups.findIndex(m => m.id === id)
        // copy the original array
        const updatedMeetups = [...meetups]
        // replace the isFavourite in the index of the array
        updatedMeetups[meetupIndex] = updatedMeetup
        // override the original array
        meetups = updatedMeetups
      }

    <MeetupGrid {meetups} on:togglefavorite={toggleFavorite} />
```

### Utilizing Slots

Badge.svelte:

```
    <style>
      span {
        display: inline-block;
        margin: 0 0.25rem;
        border-radius: 3px;
        border: 1px solid #cf0056;
        background: #cf0056;
        color: white;
        padding: 0 0.5rem;
        font-family: "Lato", sans-serif;
        font-size: 0.8rem;
      }
    </style>

    <span>
      <slot />
    </span>
```

MeetUpItem.svelte:

```
    <article>
      <header>
        <h1>
          {title}
          {#if isFav}
            <Badge>FAVORITE</Badge>
          {/if}
        </h1>
        <h2>{subtitle}</h2>
        <p>{address}</p>
      </header>
      <div class="image">
        <img src={imageUrl} alt={title} />
      </div>
      <div class="content">
        <p>{description}</p>
      </div>
      <footer>
        <Button href="mailto:{email}" caption="Contact" />
        <Button mode="outline" color={isFav ? null : "success"} caption={isFav ? "Unfavorite" : "Favorite"} on:click={() => dispatch("togglefavorite", id)} />
        <Button caption="Show Details" />
      </footer>
    </article>
```

### Communicating Between Components

EditMeetup.svelte:

```
    <script>
      import { createEventDispatcher } from "svelte";
      import TextInput from "../UI/TextInput.svelte";
      import Button from "../UI/Button.svelte";

      let title = "";
      let subtitle = "";
      let address = "";
      let email = "";
      let description = "";
      let imageUrl = "";

      const dispatch = createEventDispatcher();

      function submitForm() {
        dispatch("save", {
          title: title,
          subtitle: subtitle,
          address: address,
          email: email,
          description: description,
          imageUrl: imageUrl
        });
      }
    </script>
```

App.svelte:

```
    <script>
      let editMode

      function addMeetup(event) {
        const newMeetup = {
          id: Math.random().toString(),
          title: event.detail.title,
          subtitle: event.detail.subtitle,
          description: event.detail.description,
          imageUrl: event.detail.imageUrl,
          contactEmail: event.detail.email,
          address: event.detail.address
        }

        // meetups.push(newMeetup); // DOES NOT WORK!
        meetups = [newMeetup, ...meetups]
        editMode = null
      }
    </script>

    <main>
      <div class="meetup-controls">
        <Button caption="New Meetup" on:click={() => (editMode = "add")} />
      </div>
      {#if editMode === "add"}
        <EditMeetup on:save={addMeetup} />
      {/if}
      <MeetupGrid {meetups} on:togglefavorite={toggleFavorite} />
    </main>
```

### Creating a Re-Usable "Modal" component

Modal.svelte:

```
    <script>
      import { createEventDispatcher } from "svelte"
      import Button from "./Button.svelte"

      export let title

      const dispatch = createEventDispatcher()

      function closeModal() {
        dispatch("cancel")
      }
    </script>

    <div class="modal-backdrop" on:click={closeModal} />

    <div class="modal">
      <h1>{title}</h1>
      <div class="content">
        <slot />
      </div>
      <footer>
        <slot name="footer">
          <Button on:click={closeModal}>Close</Button>
        </slot>
      </footer>
    </div>

```

EditMeetup.svelte:

```
    <script>
      import { createEventDispatcher } from "svelte"
      import TextInput from "../UI/TextInput.svelte"
      import Button from "../UI/Button.svelte"
      import Modal from "../UI/Modal.svelte"

      let title = ""
      let subtitle = ""
      let address = ""
      let email = ""
      let description = ""
      let imageUrl = ""

      const dispatch = createEventDispatcher()

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
        <TextInput id="title" label="Title" value={title} on:input={event => (title = event.target.value)} />
        <TextInput id="subtitle" label="Subtitle" value={subtitle} on:input={event => (subtitle = event.target.value)} />
        <TextInput id="address" label="Address" value={address} on:input={event => (address = event.target.value)} />
        <TextInput id="imageUrl" label="Image URL" value={imageUrl} on:input={event => (imageUrl = event.target.value)} />
        <TextInput id="email" label="E-Mail" type="email" value={email} on:input={event => (email = event.target.value)} />
        <TextInput id="description" label="Description" controlType="textarea" value={description} on:input={event => (description = event.target.value)} />
        <!-- <Button type="submit">Save</Button> -->
      </form>

      <div slot="footer">
        <Button type="button" mode="outline" on:click={cancel}>Cancel</Button>
        <Button type="button" on:click={submitForm}>Save</Button>
      </div>
    </Modal>
```

App.svelte:

```
    <script>
      import Header from "./UI/Header.svelte"
      import MeetupGrid from "./Meetups/MeetupGrid.svelte"
      import TextInput from "./UI/TextInput.svelte"
      import Button from "./UI/Button.svelte"
      import EditMeetup from "./Meetups/EditMeetup.svelte"

      let meetups = [
        {
          id: "m1",
          title: "Coding Bootcamp",
          subtitle: "Learn to code in 2 hours",
          description: "In this meetup, we will have some experts that teach you how to code!",
          imageUrl:
            "https://upload.wikimedia.org/wikipedia/commons/thumb/9/9a/Caffe_Nero_coffee_bar%2C_High_St%2C_Sutton%2C_Surrey%2C_Greater_London.JPG/800px-Caffe_Nero_coffee_bar%2C_High_St%2C_Sutton%2C_Surrey%2C_Greater_London.JPG",
          address: "27th Nerd Road, 32523 New York",
          contactEmail: "code@test.com",
          isFavorite: false
        },
        {
          id: "m2",
          title: "Swim Together",
          subtitle: "Let's go for some swimming",
          description: "We will simply swim some rounds!",
          imageUrl: "https://upload.wikimedia.org/wikipedia/commons/thumb/6/69/Olympic_swimming_pool_%28Tbilisi%29.jpg/800px-Olympic_swimming_pool_%28Tbilisi%29.jpg",
          address: "27th Nerd Road, 32523 New York",
          contactEmail: "swim@test.com",
          isFavorite: false
        }
      ]

      let editMode

      function addMeetup(event) {
        const newMeetup = {
          id: Math.random().toString(),
          title: event.detail.title,
          subtitle: event.detail.subtitle,
          description: event.detail.description,
          imageUrl: event.detail.imageUrl,
          contactEmail: event.detail.email,
          address: event.detail.address
        }

        // meetups.push(newMeetup); // DOES NOT WORK!
        meetups = [newMeetup, ...meetups]
        editMode = null
      }

      function cancelEdit() {
        editMode = null
      }

      function toggleFavorite(event) {
        const id = event.detail
        const updatedMeetup = { ...meetups.find(m => m.id === id) }
        updatedMeetup.isFavorite = !updatedMeetup.isFavorite
        const meetupIndex = meetups.findIndex(m => m.id === id)
        const updatedMeetups = [...meetups]
        updatedMeetups[meetupIndex] = updatedMeetup
        meetups = updatedMeetups
      }
    </script>

    <Header />

    <main>
      <div class="meetup-controls">
        <Button on:click={() => (editMode = "add")}>New Meetup</Button>
      </div>
      {#if editMode === "add"}
        <EditMeetup on:save={addMeetup} on:cancel={cancelEdit} />
      {/if}
      <MeetupGrid {meetups} on:togglefavorite={toggleFavorite} />
    </main>
```
