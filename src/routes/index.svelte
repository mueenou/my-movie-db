<script>
	import MovieCard from '../components/MovieCard.svelte';
	import { onMount } from 'svelte';

	let movies = [];
	let filteringOption = null;

	let filterTabs = [
		{
			name: 'All',
			isActive: false,
			genre_id: null
		},
		{
			name: 'Horror',
			isActive: false,
			genre_id: 27
		},
		{
			name: 'Romance',
			isActive: false,
			genre_id: 10749
		},
		{
			name: 'Sci-Fi',
			isActive: false,
			genre_id: 878
		},
		{
			name: 'Thriller',
			isActive: false,
			genre_id: 53
		},
		{
			name: 'Action',
			isActive: false,
			genre_id: 28
		},
		{
			name: 'Comedy',
			isActive: false,
			genre_id: 35
		}
	];

	console.log(filterTabs);

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
	});

	$: filteredMovies = movies.filter((m) =>
		filteringOption ? m.genre_ids.includes(filteringOption) : [...movies]
	);
</script>

<div>
	<h1 class="text-center text-xl text-black">
		This app will show the most popular shows of the moment in order to help you chose what you want
		to watch and follow the latest out
	</h1>

	<div class="tabs tabs-boxed mt-10 mx-auto flex flex-row justify-center w-[300px] md:w-[530px]">
		{#each filterTabs as tab, i}
			<!-- svelte-ignore a11y-missing-attribute -->
			<a class:tab-active={tab.isActive === true} class="tab" on:click={() => tabIsActive(tab, i)}
				>{tab.name}</a
			>
		{/each}
	</div>

	{#if filteredMovies.length > 0}
		<div
			class="flex flex-col md:flex-row md:flex-wrap md:gap-x-4 md:justify-center items-center mt-10 gap-y-4"
		>
			{#each filteredMovies as movie, i}
				<MovieCard {movie} />
			{/each}
		</div>
	{/if}
</div>
