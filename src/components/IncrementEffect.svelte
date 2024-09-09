<script lang="ts">
	import Icon from "@iconify/svelte";
    import { createEventDispatcher } from "svelte";
    const dispatch = createEventDispatcher();

    export let value: number;
    export let min: number;
    export let max: number;
    export let increment: number;

    $: {
        dispatch("octaveChange", {value})
    }

    $: if (value==0) {
        dispatch("octaveChange", {value})
    }


    function increase() {
        if (value == max) {
            return;
        }
        value += increment;
    }

    function decrease() {
        if (value == min) {
            return;
        }
        value -= increment;
    }


</script>





<div class="flex flex-row items-center justify-center h-full">
    
    <button on:click={decrease}>
        <Icon style="font-size: 30px; color: rgb(147 51 234 / var(--tw-bg-opacity));" icon="mingcute:down-fill"/>
    </button>
    <div>{value}</div>
    <button on:click={increase}>
        <Icon style="font-size: 30px; color: rgb(147 51 234 / var(--tw-bg-opacity));" icon="mingcute:up-fill"/>
    </button>
</div>