<script>
	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href="/favicon.svg" />
	<title>Digital Broker</title>
	<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=0"/> 
	<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;700;800&display=swap" rel="stylesheet">
</svelte:head>

<main>
	{@render children()}
</main>

<style>
	:global(body) {
		margin: 0;
		padding: 0;
		overflow: hidden;
		font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
	}
</style>
