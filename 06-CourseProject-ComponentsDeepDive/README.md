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
