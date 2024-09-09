<script lang="ts">

    import { createEventDispatcher } from 'svelte';
    
    export let id: string;
    export let currFilter: string;
    
    const dispatch = createEventDispatcher();

    function handleClick() {
        currFilter = id;
        dispatch('filterChange', { currFilter });
    }

    


</script>




<button on:click={handleClick} 
    class="flex flex-row justify-evenly items-center w-1/3 pr-1 h-full rounded-md 
    {currFilter==id ?
    "opacity-100 transition-transform scale-110" 
    : "opacity-50 transition-transform scale-90"}" 
    id={id}>
    <slot></slot>
</button>