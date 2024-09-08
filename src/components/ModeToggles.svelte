<script lang="ts">
	import Icon from "@iconify/svelte";
    import { createEventDispatcher } from "svelte";

    const  dispatch = createEventDispatcher();
    export let noteOn: boolean;
    export let controlOn: boolean;


    $: if (noteOn) {
        dispatch("noteChange", {noteOn})
    }

    $: if (controlOn) {
        dispatch("controlChange", {controlOn})
    }

</script>



<div class="flex flex-row gap-1 w-4/5 h-full">
    <button class="flex flex-row justify-center items-center w-1/2 h-full border-2 {noteOn ? "bg-purple-200 border-0" : ""}
     border-purple-400 rounded-md" on:click={() => {noteOn = !noteOn} }>
        <Icon style="font-size: 20px; color: rgb(147 51 234 / var(--tw-bg-opacity));" icon="ph:music-note-fill"/>
    </button>

    <button class="flex flex-row justify-center items-center w-1/2 h-full border-2 {controlOn ? "bg-purple-200 border-0" : ""}
     border-purple-400 rounded-md" on:click={() => {controlOn = !controlOn}}>
        <Icon style="font-size: 20px; color: rgb(147 51 234 / var(--tw-bg-opacity));" icon="tabler:letter-a"/>
    </button>
</div>