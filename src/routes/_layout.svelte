<script>
	// import Nav from '../components/Nav.svelte';
	// export let segment;
	import Header from '../components/UI/Header.svelte'
	let mainTitle = 'CodeUp'
</script>

<svelte:head>
	<title>{mainTitle}</title>
</svelte:head>


<Header title="Welcome to {mainTitle}" />

<main>
	<slot></slot>
</main>
<style>
	main {
		margin-top: 5rem;
	}
</style>

<!-- <Nav {segment}/> -->