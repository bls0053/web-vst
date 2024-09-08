<script lang="ts">
	import Icon from "@iconify/svelte";
    import { createEventDispatcher } from "svelte";
    const dispatch = createEventDispatcher();

    export let currOctave: number = 0;
    let range: number[] = [-2, -1, 0, 1, 2];

    $: {
        dispatch("octaveChange", {currOctave})
    }

    $: if (currOctave==0) {
        dispatch("octaveChange", {currOctave})
    }


    function increment() {
        if (currOctave == range[4]) {
            return;
        }
        currOctave++;
    }

    function decrement() {
        if (currOctave == range[0]) {
            return;
        }
        currOctave--;
    }


</script>





<div class="flex flex-row justify-between w-4/5">
    
    <button on:click={decrement}>
        <Icon style="font-size: 30px; color: rgb(147 51 234 / var(--tw-bg-opacity));" icon="mingcute:down-fill"/>
    </button>
    <div>{currOctave}</div>
    <button on:click={increment}>
        <Icon style="font-size: 30px; color: rgb(147 51 234 / var(--tw-bg-opacity));" icon="mingcute:up-fill"/>
    </button>
</div>