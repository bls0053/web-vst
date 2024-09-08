<script lang="ts">
    export let min: number = 0;
    export let max: number = 100;   
    export let value: number = 50;  
    export let step: number = 1;  

    function updateValue(event: Event) {
        const target = event.target as HTMLInputElement;
        value = parseFloat(target.value);
    }

    function reset() {
        value = max/2;
    }

</script>

<div class="slider-container">
    <input 
        type="range" 
        min={min} 
        max={max} 
        step={step}
        bind:value={value} 
        on:input={updateValue}
        on:mouseup={reset}
        class="slider"
    />
</div>

<style>
    .slider-container {
        display: flex;
        flex-direction: column;
        align-items: center;
    }

    .slider {
        -webkit-appearance: none;
        width: 100%;
        height: 8px;
        background: rgb(147 51 234 / var(--tw-bg-opacity));
        border-radius: 5px;
        outline: none;
        margin: 10px 0;
    }

    .slider::-webkit-slider-thumb {
        -webkit-appearance: none;
        appearance: none;
        width: 20px;
        height: 20px;
        background: rgb(147 51 234 / var(--tw-bg-opacity));
        cursor: pointer;
        border-radius: 50%;
    }

    .slider::-moz-range-thumb {
        width: 20px;
        height: 20px;
        background: rgb(147 51 234 / var(--tw-bg-opacity));
        cursor: pointer;
        border-radius: 50%;
    }
</style>
