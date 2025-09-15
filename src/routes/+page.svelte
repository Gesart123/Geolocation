<script>
	import { onMount } from 'svelte';

	let latitude = '';
	let longitude = '';
	let accuracy = '';
	let errorText = '';

	function showPosition(position) {
		latitude  = position.coords.latitude;
		longitude = position.coords.longitude;
		accuracy  = position.coords.accuracy;
	}

	function showError(error) {
		errorText = error.message;
	}

	onMount(() => {
		if ('geolocation' in navigator) {
			navigator.geolocation.getCurrentPosition(showPosition, showError);
		} else {
			errorText = 'Geolocation wird nicht unterstützt.';
		}
	});
</script>

<h1>Standort</h1>

{#if errorText}
	<p>Fehler: {errorText}</p>
{:else if latitude}
	<p>Breitengrad: {latitude}</p>
	<p>Längengrad: {longitude}</p>
	<p>Genauigkeit (m): {accuracy}</p>
{:else}
	<p>Lade Standortdaten …</p>
{/if}
