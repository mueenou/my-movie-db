<script>
	import { onMount } from 'svelte';
	import { page } from '$app/stores';
	import { fly } from 'svelte/transition';

	let movie = undefined;

	onMount(async () => {
		const res = await fetch(
			`https://api.themoviedb.org/3/movie/${$page.params.id}?api_key=aeae1288904d0de30a4bd9f68d9a6e2f&language=en-US`
		);
		movie = await res.json();
		const moreRes = await fetch(
			`https://api.themoviedb.org/3/movie/${$page.params.id}/credits?api_key=aeae1288904d0de30a4bd9f68d9a6e2f&language=en-US`
		);
		const moreResJson = moreRes.json();
		movie.details = await moreResJson;
	});
</script>

<div>
	{#if movie != undefined}
		<h1 class="text-center text-xl text-white w-[fit-content] mx-auto rounded">
			More about: <span class="font-bold">{movie.title}</span>
		</h1>
		<div
			in:fly={{ y: 200, duration: 300 }}
			class="flex items-center h-auto lg:h-screen flex-wrap mx-auto my-32 lg:my-0"
		>
			<!--Main Col-->
			<div
				id="profile"
				class="w-full lg:w-3/5 rounded-lg lg:rounded-l-lg lg:rounded-r-none shadow-2xl bg-white opacity-75 mx-6 lg:mx-0"
			>
				<div class="p-4 md:p-12 text-center lg:text-left">
					<!-- Image for mobile view-->
					<div
						class="block lg:hidden shadow-xl mx-auto -mt-16 h-48 w-48 bg-cover bg-center"
						style="background-image: url('https://image.tmdb.org/t/p/w500/{movie.poster_path}');"
					/>

					<h1 class="text-3xl font-bold pt-8 lg:pt-0">{movie.title}</h1>
					<span
						class="ring ring-black text-xl rounded-full p-1 font-bold float-right h-9 w-9 flex flex-row justify-center items-center"
						>{movie.vote_average.toFixed(1)}</span
					>
					<div class="mx-auto lg:mx-0 w-4/5 pt-3 border-b-2 border-pink-500 opacity-25" />
					{#if movie.details}
						<p
							class="pt-2 text-gray-600 text-xs lg:text-sm flex items-center justify-center lg:justify-start"
						>
							<i class="bx bx-male-female bicon-xl mr-2 text-pink-600" />
							{#each movie.details.cast.slice(0, 3) as actor}
								<div class="mr-4">
									{actor.name}
								</div>
							{/each}
						</p>
					{/if}
					<p
						class="pt-2 text-gray-600 text-xs lg:text-sm flex items-center justify-center lg:justify-start"
					>
						<i class="bx bx-globe bicon-xl mr-2 text-pink-600" />
						{#each movie.production_countries as country}
							<div class="mr-4">
								{country.name}
							</div>
						{/each}
					</p>

					<p class="pt-8 text-sm">
						{movie.overview}
					</p>

					<div class="pt-12 pb-8">
						<a href={'https://www.imdb.com/title/' + movie.imdb_id} target="_blank">
							<button
								class="bg-pink-700 hover:bg-pink-900 text-white font-bold py-2 px-4 rounded-full"
							>
								IMDb page
							</button>
						</a>
					</div>
				</div>
			</div>

			<!--Img Col-->
			<div class="w-full lg:w-2/5">
				<img
					src={`https://image.tmdb.org/t/p/w500/${movie.poster_path || ''}`}
					alt="show poster"
					class="rounded-none lg:rounded-lg shadow-2xl hidden lg:block"
				/>
			</div>
		</div>
	{/if}
</div>
