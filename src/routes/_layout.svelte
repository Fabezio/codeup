<script>
	// import Nav from '../components/Nav.svelte';
	// export let segment;
	import Header from '../components/UI/Header.svelte'
	let mainTitle = CodeUp
</script>

<svelte:head>
	<title>{mainTitle}</title>
</svelte:head>


<Header title="Welcome to {mainTitle}" />

<main>
	<slot></slot>
</main>
<style>
	/* main {
		position: relative;
		max-width: 56em;
		background-color: white;
		padding: 2em;
		margin: 0 auto;
		box-sizing: border-box;
	} */
</style>

<!-- <Nav {segment}/> -->