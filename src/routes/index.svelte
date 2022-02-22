<script>
	import MovieCard from '../components/MovieCard.svelte';
	import { onMount } from 'svelte';
	import { page } from '$app/stores';

	let movies = [];
	let filteringOption = null;
	let allGenres = [];
	let genreIds = [];

	let filterTabs = [
		{
			genre_id: null,
			name: 'All',
			isActive: true
		}
	];

	function tabIsActive(tab, i) {
		filteringOption = tab.genre_id;
		filterTabs = filterTabs.map((t) => {
			return {
				...t,
				isActive: false
			};
		});
		filterTabs = filterTabs.map((t, index) => {
			if (i === index) {
				return {
					...t,
					isActive: t.isActive === true ? false : true
				};
			} else return t;
		});

		console.log(filterTabs);
	}

	onMount(async () => {
		const res = await fetch(
			`https://api.themoviedb.org/3/movie/popular?api_key=aeae1288904d0de30a4bd9f68d9a6e2f&language=en-US&page=1`
		);
		const jsonResponse = await res.json();
		movies = await jsonResponse.results;
		const genreRes = await fetch(
			'https://api.themoviedb.org/3/genre/movie/list?api_key=aeae1288904d0de30a4bd9f68d9a6e2f&language=en-US'
		);
		allGenres = await genreRes.json();
		allGenres = await allGenres.genres;
		console.log(movies);
		console.log('genres', allGenres);
		await movies.forEach((movie) => {
			genreIds.push(movie.genre_ids);
		});
		genreIds = genreIds.flat();
		genreIds = [...new Set(genreIds)];
		await genreIds.forEach((g) => {
			// const interTabs = [];
			allGenres.forEach((genre) => {
				if (genre.id === g) {
					// filterTabs.push({ id: g, name: genre.name, isActive: false });
					filterTabs = [...filterTabs, { genre_id: g, name: genre.name, isActive: false }];
				}
			});
		});
		console.log(filterTabs);
	});

	$: filteredMovies = movies.filter((m) =>
		filteringOption ? m.genre_ids.includes(filteringOption) : [...movies]
	);
</script>

<div>
	<h1 class="text-center text-xl text-black font-bold">Popular shows of the moment</h1>

	<div
		class="tabs tabs-boxed mt-10 mx-auto flex flex-row justify-center w-[300px] md:w-[513px] lg:w-[800px] xl:w-full"
	>
		{#each filterTabs as tab, i}
			<!-- svelte-ignore a11y-missing-attribute -->
			<a class:tab-active={tab.isActive === true} class="tab" on:click={() => tabIsActive(tab, i)}
				>{tab.name}</a
			>
		{/each}
	</div>

	{#if filteredMovies.length > 0}
		<div class="md:grid grid-cols-2 md:grid-cols-2 lg:grid-cols-4 mt-10">
			{#each filteredMovies as movie, i}
				<MovieCard {movie} />
			{/each}
		</div>
	{/if}
</div>
