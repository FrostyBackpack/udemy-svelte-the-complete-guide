### Main Goals of CourseProject

- Practice core syntax
- Get a feeling for components
- Learn about different kinds of components
- Apply concepts in more realistic environment

### Different Ways of Mounting Components

This is a great approach if you are **not building a SPA** but want to add some **"widget-like"** elements in your page.
index.html:

```
    <body>
      <div id="header"></div>
      <div id="app"></div>
    </body>
```

main.js:

```
    import App from './App.svelte';
    import Header from './UI/Header.svelte';

    const app = new App({
      target: document.querySelector("#app")
    });

    const header = new Header({
      target: document.querySelector("#header)
    });

    export default app;
```

### How to Embed Widgets

We can embed Svelte components instead of creating an entire "Svelte-only" app.
The core idea behind this approach is simply not control your entire page via Svelte components but only parts of the page.
Why and where would we maybe use such an approach?

- Existing (server-side rendered) pages that we do not want to turn in Svelte Single-Page-Apps - instead we might want to add some dynamic elements (e.g. a Svelte-controlled Dropdown button)
- Web apps (SPA or not) that are generally controlled via another framework or library (e.g. via React) - we could still take over control of a part of the page via Svelte components

However, most apps will be Single Page Apps (i.e. we control the entire page via Svelte) - it is still worth noting that we are not forced to build such an app though.

### Passing Data into Components

App.svelte:

```
    <script>
      import Header from "./UI/Header.svelte"
      import MeetUpItem from "./Meetups/MeetUpItem.svelte"

      const meetups = [
        {
          id: "m1",
          title: "Coding Bootcamp",
          subtitle: "Learn to code in 2 hours",
          description: "In this meetup, we will have some experts that teach you how to code!",
          imageUrl:
            "https://upload.wikimedia.org/wikipedia/commons/thumb/9/9a/Caffe_Nero_coffee_bar%2C_High_St%2C_Sutton%2C_Surrey%2C_Greater_London.JPG/800px-Caffe_Nero_coffee_bar%2C_High_St%2C_Sutton%2C_Surrey%2C_Greater_London.JPG",
          address: "27th Nerd Road, 32523 New York",
          contactEmail: "code@test.com"
        },
        {
          id: "m2",
          title: "Swim Together",
          subtitle: "Let's go for some swimming",
          description: "We will simply swim some rounds!",
          imageUrl: "https://upload.wikimedia.org/wikipedia/commons/thumb/6/69/Olympic_swimming_pool_%28Tbilisi%29.jpg/800px-Olympic_swimming_pool_%28Tbilisi%29.jpg",
          address: "27th Nerd Road, 32523 New York",
          contactEmail: "swim@test.com"
        }
      ]
    </script>

    <section id="meetups">
      {#each meetups as meetup}
        <MeetUpItem title={meetup.title} subtitle={meetup.subtitle} description={meetup.description} imageUrl={meetup.imageUrl} email={meetup.contactEmail} address={meetup.address} />
      {/each}
    </section>
```

MeetUpItem.svelte:

```
    <script>
      export let title
      export let subtitle
      export let imageUrl
      export let description
      export let address
      export let email
    </script>

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
        <a href="mailto:{email}">Contact</a>
        <button>Show Details</button>
        <button>Favorite</button>
      </footer>
    </article>
```
