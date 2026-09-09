<script>
	import './layout.css';

	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" type="image/png" href="/ltts.png" />
</svelte:head>

{@render children()}
