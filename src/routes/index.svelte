<script>
  import { createEventDispatcher, onMount, onDestroy } from 'svelte'
  import { fly, slide , scale} from 'svelte/transition'
  import { flip } from 'svelte/animate'
  
  import codeups from '../codeup-store'
  import Item from '../components/Codeups/Item.svelte'
  import Edit from '../components/Codeups/Edit.svelte'
  import Filter from '../components/Codeups/Filter.svelte'

  import Button from '../components/UI/Button.svelte'
  import Spinner from '../components/UI/LoadingSpinner.svelte'


  const dispatch = createEventDispatcher()
  let myUrl = 'https://codeups.firebaseio.com/codeups.json'
  let fetchedCodeups = []
  let editMode
	let editedId
  let isLoading
  let favsOnly = false
  let unsubscribe
  // let filteredCodeups

  onMount(() => {
    unsubscribe = codeups.subscribe(items => {
      fetchedCodeups = items
    })
    isLoading = true
    fetch(myUrl)
    .then( res => {
      if(!res.ok) throw new Error('Oops, Couldn\'t fetch data, please come back later...')
      console.log('Data fetched on', myUrl)
      return res.json()
    })
    .then( data => {
      const loadedCodeups = []
      for(const key in  data) {
        loadedCodeups.push({
          ...data[key],
          id: key
        })
        setTimeout(() => {
          isLoading = false
          codeups.setCodeups(loadedCodeups.reverse())
          // codeups.setCodeups([])
        }, 750 )
      }
    })
    .catch( err => {
      isLoading = false
      error = err
      console.log(err)
    })
  })

  onDestroy(() => {
    if (unsubscribe) unsubscribe()
  })
  


  $: filteredCodeups = favsOnly ? fetchedCodeups.filter(m => m.isFavorite) : fetchedCodeups ;
  // $:  console.log (codeups.isFavorite)
      
  function setFilter(event) {
    return favsOnly = event.detail === 1
  }
  function savedCodeup(event) {
		editMode = null
		editedId = null
	}		
	
	function cancelEdit () {
		editMode = null
		editedId = null
	}
	
	function startEdit (event) {
		editMode = 'edit'
		editedId = event.detail
  }	
  
  // $: console.log(codeups)
	
</script>

<svelte:head>
  <title>All Codeups</title>
</svelte:head>

{#if editMode === 'edit'}
  <Edit 
    {myUrl}
    id={editedId} on:save={savedCodeup} on:cancel={cancelEdit} 
  />
{/if}

{#if isLoading}
  <Spinner />
{:else}
  <section class="codeup-controls">
    <Filter on:select={setFilter} />
    <Button on:click={() => dispatch('add') }>new codeup</Button>
  </section>
  
  {#if filteredCodeups.length === 0}
    <p id="no-codeup">No data yet, you may start adding some</p>
  {/if}

  <section id="codeups">
  {#each filteredCodeups as codeup}
    <div
      in:fly
      out:scale
      animation:flip={{duration: 1000}}
      
    >
      <Item 
        id={codeup.id}
        title={codeup.title} 
        subtitle={codeup.subtitle} 
        description={codeup.description} 
        imageUrl={codeup.imageUrl} 
        address={codeup.address} 
        email={codeup.contactEmail} 
        isFavorite={codeup.isFavorite}
        
        on:edit
        on:showdetails
        on:close
      />
    </div>
  {/each}

    </section>


{/if}


			<!-- <Grid 
				on:showdetails={showDetails} 
				codeups={$codeups} 
				on:edit={startEdit}
				on:add={() => editMode = 'edit'}
			/> -->

<style>
  #codeups {
    margin: 0 auto; 
	}
  #no-codeup {
    margin: 1rem;
  }
  /* #codeups-controls {
		width: 100%;
		text-align: center;
	} */
  .codeup-controls {
		display: flex;
    padding: 1rem; 
    justify-content: space-between;
	}

section {
  width: 100%;
  display: grid;
  margin: 0 auto;
  grid-template-columns: 1fr;
  grid-gap: 1rem;
  overflow-y: scroll;
}

@media (min-width: 768px) {
  section {
    grid-gap: 0;
    grid-template-columns: repeat(2, 1fr);
  }
}
@media (min-width: 1200px) {
  section {
    grid-gap: 0;
    grid-template-columns: repeat(3, 1fr);
  }
}

</style>