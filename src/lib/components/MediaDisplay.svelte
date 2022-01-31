<script>
    export let type;
    export let source;
</script>

{#if type === "img"}
	<div class="carousel-cell">
		<img src="{source}?$categorypage_XL$" alt="img" class="img-item" />
	</div>
{:else}
	<div class="carousel-cell video-grid">
		<video muted loop="true" preload="auto" autoplay="true" class="video-item">
			<source src="{source}/mp4_product_high">
		</video>
	</div>	
{/if}

<style>
	.img-item {
		width: 100%;
		height: 100%;
	}

	.video-item {
		grid-column-start: 2;
		grid-row-start: 1;
		width: 100%;
	}

	.video-grid {
		grid-column-start: 2;
		grid-row-start: 1;
		width: 100%;
	}
</style>

