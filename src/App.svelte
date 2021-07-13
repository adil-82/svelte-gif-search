<script>
	import Tailwindcss from './Tailwindcss.svelte';
	const API_URL = 'https://api.giphy.com/v1/gifs/search?api_key=JnxTmEGKXjZeUKBzRjTQoMDg8OX8pS5U&rating=pg&q=';

	let search = "";
	let loading = false;
	let gifs = [];
	// let promises = submitHandler();
	async function submitHandler() {
		console.log(search)
		loading = true;
		gifs = [];
		const url = ` ${API_URL}${search} `;
		const res = await fetch(url);
		const json = await res.json();
		gifs = json.data.map(gif => gif.images.fixed_height.url)
		loading = false;
		console.log(gifs)
	}
</script>

<Tailwindcss />

<main >

	<form on:submit|preventDefault={submitHandler} >
		<label for="search">Search an animal gifs</label>
		<input type="text" name="search" id="search" bind:value={search} >
		<button type="submit"> Go</button>
	</form>

</main>

	{#if loading }
		<img src="https://flevix.com/wp-content/uploads/2020/01/Preloader.gif" alt="loading">
	<!-- <p>Loading...</p>		 --> 
	{/if}

		
	{#each gifs as gif }
	<div class="result">
		<img src={gif} alt="gif">
	</div>
	{/each}


<style>
	main {
		text-align: center;
		padding: 2em;
		max-width: 240px;
		margin: 0 auto;
	}

	.result {
		column-count: 3;
	}
	img {
		width: 100%;
		height: auto;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>
