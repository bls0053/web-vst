<script lang="ts">

    import { createEventDispatcher } from 'svelte';
    
    export let id: string;
    export let currWave: string;
    
    const dispatch = createEventDispatcher();

    function handleClick() {
        currWave = id;
        dispatch('waveChange', { currWave });
    }

    


</script>




<button on:click={handleClick} 
    class="rounded-md 
    {currWave==id ?
    "opacity-100 transition-transform scale-110" 
    : "opacity-50 transition-transform scale-90"}" 
    id={id}>
    <slot></slot>
</button>