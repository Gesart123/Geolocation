<script>
	import '../app.css';
	import favicon from '$lib/assets/favicon.svg';

	let { children } = $props();
</script>

<svelte:head>
	<meta charset="utf-8" />
	<meta name="viewport" content="width=device-width, initial-scale=1" />
	<title>Geolocation Beispiel</title>
	<link rel="icon" href={favicon} />
</svelte:head>

<div class="min-h-screen flex flex-col bg-gray-50 text-gray-900">
	<header class="bg-gray-800 text-white p-4">
		<h1 class="text-lg font-bold">Geolocation Demo</h1>
	</header>

	<main class="flex-1 p-6">
		{@render children?.()}
	</main>

	<footer class="bg-gray-100 text-center p-4 text-sm text-gray-500">
		© {new Date().getFullYear()} Demo
	</footer>
</div>
