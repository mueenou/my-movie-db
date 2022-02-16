<script>
	import { onMount } from 'svelte';
	import { page } from '$app/stores';

	console.log($page.params);

	let movie = undefined;

	onMount(async () => {
		const res = await fetch(
			`https://api.themoviedb.org/3/movie/${$page.params.id}?api_key=aeae1288904d0de30a4bd9f68d9a6e2f&language=en-US`
		);
		movie = await res.json();
		console.log(movie.poster_path);
	});
</script>

<div>
	{#if movie != undefined}
		<img src={`https://image.tmdb.org/t/p/w500/${movie.poster_path || ''}`} alt="" />
	{/if}
</div>
