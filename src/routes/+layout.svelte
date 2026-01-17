<script lang="ts">
	import favicon from '$lib/assets/favicon.svg';
	import visLogo from '$lib/assets/vis.png';

	let { children } = $props();
</script>

<svelte:head>
	<title>Visualization 2026, CSCI 5609</title>
	<!-- <link rel="icon" href={favicon} /> -->
	<link rel="icon" href={visLogo} />
	<link rel="preconnect" href="https://fonts.googleapis.com" />
	<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
	<link
		href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;500;700&display=swap"
		rel="stylesheet"
	/>
</svelte:head>

{@render children()}

<style>
	:global(body) {
		margin: 0;
		font-family: 'Roboto', 'Segoe UI', sans-serif;
		color: #1b1b1b;
		background: #ffffff;
	}

	:global(html) {
		scroll-behavior: smooth;
	}
</style>
