<script>
    import { createEventDispatcher } from "svelte";
    import { selectedColor } from "../data/stores";

    export let productSku;

    const dispatch = createEventDispatcher();

    let activeSku;
    selectedColor.subscribe(x => {
        activeSku = x;
    });

</script>

<div class="picker-container">
    <a href="#" class="pdp-color-picker" on:click={() => dispatch('notify', {productSku})}>
        <img src="https:\/\/media.nelly.com\/i\/nlyscandinavia\/{productSku}_40?$colorSwatch2x$" alt="" class="img-picker {activeSku === productSku ? "selected-color" : ""}">
    </a>
</div>

<style>
    .pdp-color-picker {
        border-radius: 50%;
    }

    .picker-container {
        padding-right: 5%;
    }

    .img-picker {
        border-radius: 50%;
        width: 100%;
        padding: 5%;
    }

    .selected-color {
        border: 2px solid rgba(0,0,0,.8);
        width: 90%;
        margin: 1%;
    }

</style>