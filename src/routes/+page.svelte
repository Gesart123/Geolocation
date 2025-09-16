<script>
	import { onMount } from 'svelte';

	let address = '';
	let errorText = '';
	let loading = false;
	let flagUrl = '';

	async function fetchAddress(latitude, longitude) {
		try {
			const res = await fetch(
				`https://nominatim.openstreetmap.org/reverse?format=json&lat=${latitude}&lon=${longitude}`
			);
			if (!res.ok) throw new Error('Fehler beim Abrufen der Adressdaten.');
			const data = await res.json();

			const addr = data.address || {};
			address = [
				addr.road,
				addr.house_number,
				addr.postcode,
				addr.city || addr.town || addr.village,
				addr.state,
				addr.country
			]
				.filter(Boolean)
				.join(', ');

			if (addr.country) {
			const response = await fetch(
				`https://restcountries.com/v3.1/name/${encodeURIComponent(
					addr.country
				)}?fullText=true`
			);
			if (!response.ok) throw new Error('Error retrieving flag.');
			const countryData = await response.json();

			flagUrl = countryData[0]?.flags?.svg || '';
		}
		} catch (err) {
			errorText = err.message;
		}
	}
		
	
	function getLocation() {
		errorText = '';
		address = '';
		loading = true;

		if ('geolocation' in navigator) {
			navigator.geolocation.getCurrentPosition(
				(position) => fetchAddress(position.coords.latitude, position.coords.longitude),
				(error) => {
					errorText = error.message;
					loading = false;
				}
			);
		} else {
			errorText = 'Geolocation is not supported.';
			loading = false;
		}
	}
</script>

<h1>My Location </h1>
<button on:click={getLocation}
	class="px-4 py-2 bg-blue-600 text-white rounded hover:bg-blue-700">
	Get Location
</button>

{#if errorText}
	<p style="color:red;">Error: {errorText}</p>
{:else if address}
	<p><strong>Address:</strong> {address}</p>

{#if flagUrl}
	<p><strong>Flag:</strong></p>
	<img src={flagUrl} alt="LandFlag" style="width:150px;height:auto;" />
{/if}
{:else}
	<p>Click on “Get location” to see your address and flag.</p>
{/if}