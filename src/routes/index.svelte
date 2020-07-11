<script>
	// Modules
	import codeups from './Codeups/codeup-store'
	import Grid from './Codeups/Grid.svelte'
	import Edit from './Codeups/Edit.svelte'
	import Detail from './Codeups/Detail.svelte'
	import Header from './UI/Header.svelte'
	import Button from './UI/Button.svelte'
	import Modal from './UI/Modal.svelte'
	import Spinner from './UI/LoadingSpinner.svelte'
	import Error from './UI/Error.svelte'

	// Variables
	export let mainTitle;
	let tabTitle = 'Codeups main mage'
	let editMode
	let editedId
	let page = 'overview'
	const pageData = {}
	let myUrl = 'https://codeups.firebaseio.com/codeups.json'
	let isLoading = true
	let error
	// let codeups = codeups
	// export let id

</script>

<main>
	{#if page === 'overview'}
		
		{#if editMode === 'edit'}
			<Edit 
				{myUrl}
				id={editedId} on:save={savedCodeup} on:cancel={cancelEdit} 
			/>
		{/if}
		{#if isLoading}
			<Spinner />
		{:else}
			<Grid 
				on:showdetails={showDetails} 
				codeups={$codeups} 
				on:edit={startEdit}
				on:add={() => editMode = 'edit'}
			/>
		{/if}
	{:else }
		<Detail id={pageData.id} 	on:close={closeDetails} />
	{/if}
	{#if error}
		<Error message={error.message} on:cancel={cancelError} />
	{/if}

</main>

<style>
	
</style>
