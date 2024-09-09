<script lang="ts">

    import Icon from '@iconify/svelte';
    import { onMount } from 'svelte';


    import { createEventDispatcher } from 'svelte';

    export let textSize: number = 50;
    export let min: number = 0;
    export let max: number = 1;
    export let value: number = 0;
    export let decimals: number = 2;
    let originalValue: number;
    let rotation = 0;
    let editableDiv: HTMLDivElement;


    const dispatch = createEventDispatcher();

	export let rotRange = 2 * Math.PI * 45;
	export let pixelRange = 300;
	export let startRotation = -Math.PI * 45;
	
	let startY: number, startValue: number;
	$: valueRange = max - min;
	$: rotation = startRotation + (value - min) / valueRange * rotRange;
	
	function clamp(num: number, min: number, max: number) {
		return Math.max(min, Math.min(num, max));
	}
	
	function pointerMove({ clientY }) {
		const valueDiff = valueRange * (clientY - startY) / pixelRange;
		value = parseFloat(clamp(startValue - valueDiff, min, max).toFixed(4))
	}
	
	function pointerDown({ clientY }) {
		console.log({ clientY });
		startY = clientY;
		startValue = value;
		window.addEventListener('pointermove', pointerMove);
		window.addEventListener('pointerup', pointerUp);
	}
	
	function pointerUp() {
		window.removeEventListener('pointermove', pointerMove);
		window.removeEventListener('pointerup', pointerUp);
	}

    onMount(() => {
        
    });

    

    function handleInput(event: Event) {
        originalValue = value;
    }

    function handleKeyDown(event: KeyboardEvent) {
        if (event.key === 'Enter') {
            const newValue = parseFloat(editableDiv.innerText.trim());
            if (!isNaN(newValue)) {
                if (newValue < min) {
                    value = min;
                }
                else if (newValue > max) {
                    value = max;
                }
                else {
                    value = newValue;
                }
                rotation = -160 + ((value - min) / (max - min)) * 320;
            }
            editableDiv.blur();
        } 
        else if (event.key === 'Escape') {
            editableDiv.blur();
            return;
        }
    }

    $: if (value==0) {
            console.log(value)
            if (editableDiv) {
                editableDiv.innerText = value.toFixed(decimals).toString();
        }
    }

    $: if (value) {
            console.log(value)
            if (editableDiv) {
                editableDiv.innerText = value.toFixed(decimals).toString();
        }
    }


</script>


<div class="flex flex-row w-full items-center justify-evenly">
    <!-- svelte-ignore a11y-no-static-element-interactions -->
    <div
        on:pointerdown={pointerDown}
        class="flex items-center justify-center cursor-pointer transform transition-transform rounded-full"
        style="rotate: {rotation}deg;"
    >

        
        <Icon icon="mdi:knob" style="font-size: {textSize}; color: rgb(147 51 234 / var(--tw-bg-opacity));" />
    </div>
    <!-- svelte-ignore a11y-no-static-element-interactions -->
    <div 
        bind:this={editableDiv}
        contenteditable="true" 
        on:input={handleInput}
        on:keydown={handleKeyDown}
        class="flex flex-row justify-center w-1/2 bg-green-200 border-2 rounded-md border-green-400">
        {value}
    
    </div>

</div>
    