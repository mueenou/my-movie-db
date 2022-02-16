<script>
	export let movie;
</script>

<div class="w-[230px] h-[300px] shadow-md rounded-lg overflow-hidden">
	<img
		src={`https://image.tmdb.org/t/p/w500/${movie.backdrop_path}`}
		alt={`movie${movie.id} poster`}
		class="h-[45%] w-full"
	/>
	<div class="flex flex-col justify-between p-2 h-[55%]">
		<div>
			<span class="text-sm font-bold">{movie.title}</span>
			<p class="text-sm mt-2">{movie.overview.slice(0, 85) + '...'}</p>
		</div>
		<div class="mt-auto">
			<span class="text-xs bg-pink-100 text-slate-500 rounded-full py-1 px-2"
				>{movie.release_date}</span
			>
			<span class="text-xs bg-pink-100 text-slate-500 rounded-full py-1 px-2"
				>{movie.vote_average}</span
			>
		</div>
	</div>
</div>
