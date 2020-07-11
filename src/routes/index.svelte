<script>
	// Modules
	import codeups from '../codeup-store'
	import Grid from './Grid.svelte'
	import Detail from './Detail.svelte'
	import Edit from '../components/Codeups/Edit.svelte'
	import Header from '../components/UI/Header.svelte'
	import Button from '../components/UI/Button.svelte'
	import Modal from '../components/UI/Modal.svelte'
	import Spinner from '../components/UI/LoadingSpinner.svelte'
	import Error from '../components/UI/Error.svelte'

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
