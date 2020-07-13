<script context="module">
  export function preload(page) {
    console.log(page)
    const meetupId = page.params.id
    return this.fetch(`https://meetups-4c17d.firebaseio.com/meetups/${meetupId}.json`)
    .then(res => {
      if(!res.ok) throw new Error('Could not fetch detail')
      return res.json()
    })
    .then(meetupData => {
      return {loadedMeetup: {...meetupData, id: meetupId}}
    })
    .catch(err => {
      // error = err
      this.error(404, 'could not find detail')
      // console.log(err)
    })
  }
</script>

<script>
  // import { createEventDispatcher } from "svelte";
  import meetups from "../meetups-store.js";
  import Button from "../components/UI/Button.svelte";
  import Badge from "../components/UI/Badge.svelte";

  export let loadedMeetup;
  
</script>

<style>
  .image {
    width: 100%;
    height: 600px;
    background: #e7e7e7;
  }

  img {
    width: inherit;
    height: 100%;
    object-fit:cover;
  }
  .content {
    text-align: center;
    width: 80%;
    margin: auto;

  }
  h1 {
    font-size: 2rem;
    margin: 0.5rem 0;
  }
  h2 {
    font-size: 1.25rem;
    color: #6B6B6B;
  }
  p {
    font-size: 1.5rem;
  }
</style>

<section>
  <div class="image">
    <img src={loadedMeetup.imageUrl} alt={loadedMeetup.title} >
  </div>
  <div class="content">
    <h1>{loadedMeetup.title}</h1>
    <h2>{loadedMeetup.subtitle} {loadedMeetup.address}</h2>
    <p>{loadedMeetup.description}</p>
  </div>
  <div class="footer">
    <Button color="success" href="mailto:{loadedMeetup.contactEmail}" >Contact</Button>
    <Button type="button" mode='oulined' href="/" >close</Button>
  </div>

    
</section>
<!-- 
<article>
  <header>
    <h1>
      {loadedMeetup.title}
      {#if loadedMeetup.isFav}
        <Badge>FAVORITE</Badge>
      {/if}
    </h1>
    <h2>{loadedMeetup.subtitle}</h2>
    <p>{loadedMeetup.address}</p>
  </header>
  <div class="image">
    <img src={loadedMeetup.imageUrl} alt={loadedMeetup.title} />
  </div>
  <div class="content">
    <p>{loadedMeetup.description}</p>
  </div>
  <footer>
    <Button mode="outline" type="button" on:click={() => dispatch('edit', loadedMeetup.id)}>
      Edit
    </Button>
    <Button
      mode="outline"
      color={loadedMeetup.isFav ? null : 'success'}
      type="button"
      on:click={loadedMeetup.toggleFavorite}>
      {loadedMeetup.isFav ? 'Unfavorite' : 'Favorite'}
    </Button>
    <Button type="button" href="/">
      Show Details
    </Button>
  </footer>
</article> -->
