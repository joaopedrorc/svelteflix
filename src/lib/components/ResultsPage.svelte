<script lang="ts">
	import { media } from '$lib/api';
	import type { MovieListResult } from '$lib/types';
	import { createEventDispatcher, onMount } from 'svelte';

	export let movies: MovieListResult[];
	export let next: string | null;

	const dispatch = createEventDispatcher();

	let viewport: HTMLDivElement;
	let results: HTMLDivElement;
	let itemWidth: number = 0;
	let itemHeight: number = 0;
	let numColumns: number = 4;

	let a: number = 0;
	let b: number = movies.length;
	let paddingTop: number = 0;
	let paddingBottom: number = 0;

	function handleResize() {
		const first = results.firstChild as HTMLAnchorElement;

		itemWidth = first.offsetWidth;
		itemHeight = first.offsetHeight;
		numColumns = 4;

		handleScroll();
	}

	function handleScroll() {
		const remaining = viewport.scrollHeight - (viewport.scrollTop - viewport.clientHeight);
		console.log(remaining);

		if (remaining < 1400) {
			dispatch('end');
		}

		a = Math.floor(viewport.scrollTop / itemHeight) * numColumns;
		b = Math.min(a + Math.ceil(viewport.offsetHeight / itemHeight) * numColumns, movies.length);

		paddingTop = Math.floor((a / numColumns) * itemHeight);
		paddingBottom = Math.floor((movies.length - b) / numColumns) * itemHeight;
	}

	onMount(handleResize);
</script>

<svelte:window on:resize={handleResize} />

<p>showing items from {a} to {b}</p>

<div bind:this={viewport} class="viewport" on:scroll={handleScroll}>
	<div
		bind:this={results}
		class="results"
		style:paddingTop="{paddingTop}px"
		style:paddingBottom="{paddingBottom}px"
	>
		{#each movies.slice(a, b) as movie}
			<a href="/movies/{movie.id}">
				<img src={media(movie.poster_path, 500)} alt={movie.title} />
			</a>
		{/each}
	</div>

	{#if next}
		<a href={next}>next page</a>
	{/if}
</div>

<style>
	.viewport {
		height: 500px;
		overflow-y: auto;
	}

	.results {
		display: grid;
		grid-template-columns: repeat(4, 1fr);
		gap: 1rem;
		width: 100%;
	}

	a {
		width: 100%;
	}

	img {
		width: 100%;
		aspect-ratio: 2/3;
	}
</style>
