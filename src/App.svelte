<script>
	export let name;
	import Image from "./lib/components/Image.svelte";
	import data from "./lib/data/product-data.json";
</script>

<article>
	<section>

	</section>
	<aside>

	</aside>
	<h1>Hello {name}!</h1>
	<p>Visit the <a href="https://svelte.dev/tutorial">Svelte tutorial</a> to learn how to build Svelte apps.</p>
</article>

<style>
	article {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
		font-family: 'Montserrat', sans-serif;
	}

	@media (min-width: 640px) {
		article {
			max-width: none;
		}
	}
</style>