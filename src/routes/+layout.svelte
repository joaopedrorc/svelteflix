<script>
	import { page } from '$app/stores';
	import '../styles.css';
	import logo from '$lib/images/logo.svg';
	import themoviedblogo from '$lib/images/themoviedblogo.svg';
</script>

<nav class="column">
	<a href="/"> <img src={logo} alt="SvelteFlix Logo" /></a>

	<div class="links">
		<a href="/search">Search</a>
		<a href="/watchlist">Watch list</a>
		<a href="/login">Log in</a>
	</div>
</nav>

<main class:infinite={$page.data.infinite}>
	<slot />
</main>

<footer>
	<p>
		Data provided by <a href="https://www.themoviedb.org/" target="_blank">
			<img src={themoviedblogo} alt="The Movie DB Logo" />
		</a>
	</p>
</footer>

<style>
	nav {
		display: flex;
		width: 100%;
		height: 3rem;
		align-items: center;
		justify-content: space-between;
		max-width: var(--column);
		padding: 0 var(--side);
		color: var(--accent);
	}

	a {
		color: inherit;
		text-decoration: none;
	}

	img {
		height: 1rem;
	}

	.links {
		display: flex;
		gap: 1rem;
	}

	main.infinite {
		height: 0;
		flex: 1;
		overflow: hidden;
	}

	footer {
		display: flex;
		justify-content: center;
		height: 5rem;
	}
</style>
