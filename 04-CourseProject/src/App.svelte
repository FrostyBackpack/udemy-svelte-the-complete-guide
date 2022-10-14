<script>
  import Header from "./UI/Header.svelte"
  import MeetUpGrid from "./Meetups/MeetUpGrid.svelte"
  import TextInput from "./UI/TextInput.svelte"
  import Button from "./UI/Button.svelte"

  let title = ""
  let subtitle = ""
  let address = ""
  let email = ""
  let description = ""
  let imageUrl = ""

  let meetups = [
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

  function addMeetUp() {
    const newMeetUp = {
      id: Math.random().toString(),
      title: title,
      subtitle: subtitle,
      description: description,
      imageUrl: imageUrl,
      contactEmail: email,
      address: address
    }

    // meetups.push(newMeetup) [DOES NOT WORK!]
    // 1. meetups.push(newMeetup) 2. meetups = meetups [WORKS!]
    // array push is not a trigger that signals to use Svelte that it needs to check the DOM and potentially update the DOM
    // the trigger is the equal sign (=)
    meetups = [newMeetUp, ...meetups]
  }
</script>

<Header />

<!-- main: default HTML element -->
<main>
  <!-- preventDefault: no HTTP requests get sent instead we handle this entirely on client side -->
  <!-- bind:value={variable}: set the initial value as empty string as value for input and once typed, the entered value will be saved back into variable -->
  <form on:submit|preventDefault={addMeetUp}>
    <TextInput id="title" label="Title" type="text" value={title} on:input={event => (title = event.target.value)} />

    <TextInput id="subtitle" label="Subtitle" type="text" value={subtitle} on:input={event => (subtitle = event.target.value)} />

    <TextInput id="address" label="Address" type="text" value={address} on:input={event => (address = event.target.value)} />

    <TextInput id="imageUrl" label="Image URL" type="text" value={imageUrl} on:input={event => (imageUrl = event.target.value)} />

    <TextInput id="email" label="E-mail" type="email" value={email} on:input={event => (email = event.target.value)} />

    <TextInput id="description" label="Description" controlType="textarea" value={description} on:input={event => (description = event.target.value)} />

    <Button type="submit" caption="Save" />
  </form>
  <MeetUpGrid {meetups} />
</main>

<style>
  main {
    margin-top: 5rem;
  }

  form {
    width: 30rem;
    max-width: 90%;
    margin: auto;
  }
</style>
