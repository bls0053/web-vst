<script lang="ts">

    import Icon from '@iconify/svelte';
    import { onMount } from 'svelte';


    import { createEventDispatcher } from 'svelte';

    export let textSize: number = 50;
    export let min: number = 0;
    export let max: number = 1;
    export let value: number = 0;

    let originalValue: number;
    let rotation = 0;
    let container: HTMLDivElement;
    let lastAngle: number= 0;
    let mousePressed: boolean = false;
    const dispatch = createEventDispatcher();

    function rotationToValue(rotation: number) {
        const percentage = (rotation + 160) / 320
        return min + percentage * (max - min)
    }

    function handleMouseMove(event: MouseEvent) {
        if (!container) return;
        if (!mousePressed) return;
        const rect = container.getBoundingClientRect();
        const centerX = rect.left + rect.width / 2;
        const centerY = rect.top + rect.height / 2;
        const deltaX = event.clientX - centerX;
        const deltaY = event.clientY - centerY;

        let angle = Math.atan2(deltaY, deltaX) * (320 / Math.PI);
        angle = Math.max(-160, Math.min(160, angle));

        const angleChange = angle - lastAngle;
        if (Math.abs(angleChange) > 100) {
            return;
        }

        rotation = Math.floor(angle);
        const newValue = parseFloat(rotationToValue(rotation).toFixed(2));
        if (newValue !== value) {
            value = newValue;
            dispatch('valueChange', { value });
        }
        lastAngle = angle;
    }

    onMount(() => {
        const percentage = (value - min) / (max - min);
        rotation = -160 + (percentage * 320);
        dispatch('valueChange', { value });
    });

    function mouseDown() {
        mousePressed = true;
    }

    function mouseUp() {
        mousePressed = false;
    }

    function handleInput(event: Event) {
        originalValue = value;
    }

    function handleKeyDown(event: KeyboardEvent) {
        const target = event.target as HTMLDivElement;
        if (event.key === 'Enter') {
            const newValue = parseFloat(target.innerText.trim());
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
                target.innerText = value.toString(); 
            }
            else {
                target.innerText = value.toString(); 
            }
            dispatch('valueChange', { value });

        } 
        else if (event.key === 'Escape') {
            target.innerText = value.toString(); 
        }
    }

</script>


<div class="flex flex-row w-full items-center justify-evenly">
    <!-- svelte-ignore a11y-no-static-element-interactions -->
    <div
        bind:this={container}
        class="flex items-center justify-center cursor-pointer transform transition-transform rounded-full"
        on:mousemove={handleMouseMove}
        on:mousedown={mouseDown}
        on:mouseup={mouseUp}
        style="rotate: {rotation}deg;"
    >

        
        <Icon icon="mdi:knob" style="font-size: {textSize}; color: rgb(147 51 234 / var(--tw-bg-opacity));" />
    </div>
    <!-- svelte-ignore a11y-no-static-element-interactions -->
    <div 
        contenteditable="true" 
        on:input={handleInput}
        on:keydown={handleKeyDown}
        class="flex flex-row justify-center w-1/3 bg-green-200 border-2 rounded-md border-green-400">
        {value}
    
    </div>

</div>
    