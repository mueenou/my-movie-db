<script>
	import MovieCard from '../components/MovieCard.svelte';
	import { onMount } from 'svelte';

	let movies = [];

	onMount(async () => {
		const res = await fetch(
			`https://api.themoviedb.org/3/movie/popular?api_key=aeae1288904d0de30a4bd9f68d9a6e2f&language=en-US&page=1`
		);
		const jsonResponse = await res.json();
		movies = await jsonResponse.results;
		console.log(movies[0]);
	});
</script>

<div>
	<h1>Welcome to my movie app</h1>

	<div class="list-navigation-bar w-100 mt-10">
		<ul class="flex flex-row w-full gap-4">
			<li><a href="#">Horror</a></li>
			<li><a href="#">Action</a></li>
			<li><a href="#">Thriller</a></li>
			<li><a href="#">Romance</a></li>
			<li><a href="#">SciFi</a></li>
		</ul>
	</div>
	<!-- https://image.tmdb.org/t/p/w500/kqjL17yufvn9OVLyXYpvtyrFfak.jpg -->
	<div class="flex flex-col items-center mt-10 gap-y-4">
		{#each movies as movie, i}
			<MovieCard {movie} />
		{/each}
	</div>
</div>
