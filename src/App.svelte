<script>
	import { onMount } from "svelte";
	import ColorPicker from "./lib/components/ColorPicker.svelte";
	import MediaDisplay from "./lib/components/MediaDisplay.svelte";
	import data from "./lib/data/product-data.json";
	import { selectedColor } from "./lib/data/stores";
	import jQuery from "jquery";

	//Sort SKUs sizes
	let availableSizes = data.Sizes;
	let unavaiableSizes = data.SoldOutSizes;
	availableSizes.forEach(item => {item.available = ""});
	unavaiableSizes.forEach(item => {item.available = "sold"});
	
	let sizeMenu = availableSizes.concat(unavaiableSizes);
	sizeMenu.sort((a, b) => {
		return a.Id - b.Id;
	});

	let colorPickerItems = data.ColorPickerItems;
	
	let descriptionList = data.Description.split('\u003cbr /\u003e');

	let flickityProps = '{ "watchCSS":true, "wrapAround":true, "prevNextButtons":false}';

	let images = [];
	onMount(() => {
		getProductData("091323-1126");

		//DROPDOWN easy fix
		jQuery(".dropdown-size").on("click", ".init", function() {
			jQuery(this).closest(".dropdown-size").children('li:not(.init)').toggle();
		});

		let allOptions = jQuery(".dropdown-size").children('li:not(.init)');
		jQuery(".dropdown-size").on("click", "li:not(.init)", function() {
			allOptions.removeClass('selected');
			jQuery(this).addClass('selected');
			jQuery(".dropdown-size").children('.init').html(jQuery(this).html());
			allOptions.toggle();
		});
	});

	async function getProductData(product) {
		let productSku = typeof(product) === 'object' ? product.detail.productSku : product;
		images = [];
		let req;
		const res = await fetch(`https://media.nelly.com/s/nlyscandinavia/${productSku}.json`);
		req = await res.json();
		images = req.items;
		images.splice(images.length - 2, 1);

		selectedColor.update(() => productSku );

		//fix flickity content redraw
		if (innerWidth <= 1024) {
			setTimeout(() => {
				let flkty = Flickity.data('.carousel');
				flkty.deactivate();
				flkty.activate();
			}, 200);
		}
	}
</script>

<article>
	<span class="about">Kläder >> Festklänningar >> Maxiklänningar</span>
	<section class="pdp">
		<span class="icon-share"></span>
		<span class="icon-button"><span class="icon-heart"></span></span>
		<span class="icon-button-play"><span class="icon-video"></span></span>
		<div class="carousel pdp-media" data-flickity={flickityProps}>
			{#each images as image}
				<MediaDisplay type="{image.type}" source="{image.src}"/> 
			{/each}
		</div>
	</section>
	<section class="pdp-info">
		<div class="pdp-title-container">
			<div>
				<div>
					<a href="https://nelly.com{data.Link}">{data.BrandName}</a>
				</div>
				<h1>{data.Name}</h1>
			</div>
			<div class="pdp-price-container">
				<div>
					<span class="discount-price">{data.Discount}</span>
					<span class="ordinary-price">{data.OrdinaryPrice}</span>
				</div>
				<div>
					<div class="price">{data.Price}</div>
				</div>
			</div>
		</div>
	<div class="action-container">
		<div class="dropdown-wrapper">
			<ul class="dropdown-size">
				<li class="init">Välj storlek</li>
				{#each sizeMenu as { Id, Name, available }}
				<li>{Name} {available}</li>
				{/each}
			</ul>
		</div>
		<div class="buy-fav-buttons">
			<a href="#" class="add-button">Lägg till</a>
			<a href="#" class="heart-button"></a>
		</div>
	</div>
		<div class="selected-color">
			<span>Vald färg: <strong>Burgundy</strong></span>
		</div>
		<div class="picker-wrapper">
			<ColorPicker productSku={data.ShortSku} on:notify="{getProductData}"/>
			{#each colorPickerItems as { ShortSku }}
			<ColorPicker productSku={ShortSku.AsString} on:notify="{getProductData}"/>
			{/each}
		</div>
		<div>
			<h4>BESKRIVNING</h4>
			<ul class="description-list">
				{#each descriptionList as description }
				<li>{ description }</li>
				{/each}
			</ul>
		</div>

	</section>
</article>

<style>

</style>