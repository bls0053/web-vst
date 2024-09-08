<script lang="ts">
    import { Button } from "$lib/components/ui/button";
	import { onMount } from "svelte";
	import { cubicInOut, linear } from "svelte/easing";
	import { tweened } from "svelte/motion";
	import { fade, fly } from "svelte/transition";

    export let note: string;
    export let frequency: number;
    export let keypress: string;
    export let masterGain: GainNode;
    export let audioContext: AudioContext;
    export let isBlackKey: boolean;
    export let adsr: number[];
    export let wave: string;
    export let octave: number;
    export let controlOn: boolean;
    export let noteOn: boolean;
    export let pitch: number;
    export let filter: string;

    let attack: number;
    let decay: number;
    let sustain: number;
    let release: number;

    

    $: if (adsr) {
        attack = adsr[0];
        decay = adsr[1];
        sustain = adsr[2];
        release = adsr[3];
    }

    let mousePressed: boolean = false;
    let mouseEntered: boolean = false;
    let keyHeld: boolean = false;
    let init: boolean = false;
    let bool: boolean = false;

    let oscillator: OscillatorNode;
    let frequencyParam: AudioParam;
    let gainNode: GainNode;
    let filterNode: BiquadFilterNode;
    
    function applyADSR(gainNode: GainNode) {
        const now = audioContext.currentTime;
        gainNode.gain.setValueAtTime(0, now);
        gainNode.gain.linearRampToValueAtTime(1, now + attack);
        gainNode.gain.linearRampToValueAtTime(sustain, now + attack + decay);
    }

	function playNote() {
        oscillator = audioContext.createOscillator();
        filterNode = audioContext.createBiquadFilter();
        
        const now = audioContext.currentTime;
        frequencyParam = oscillator.frequency;
        
        filterNode.type = filter;
        filterNode.Q.setValueAtTime(1, now)
        oscillator.type = wave;
        
        updateFilter()
        updateFrequency(pitch);
        gainNode = audioContext.createGain();
        applyADSR(gainNode);

        oscillator.connect(filterNode);
        filterNode.connect(gainNode)
        gainNode.connect(masterGain);
        
        oscillator.start();
        init = true;
    }

    $: if (pitch || octave) {
        updateFrequency(pitch)
    }

    $: if (octave == 0) {
        updateFrequency(pitch)
    }

    $: if (wave) {
        updateWave();
    }

    $: if (filter) {
        console.log(filter)
        updateFilter(); 
    }

    function updateFilter() {
        if (filterNode) {
            filterNode.type = filter;
        }
    }

    function updateWave() {
        if (oscillator) {
            oscillator.type = wave;
        }
    }

    function updateFrequency(newPitch: number) {
        if (oscillator) {
            const now = audioContext.currentTime;
            frequencyParam.setValueAtTime(((frequency + ((newPitch-.48)*((frequency)*(2**(1/12))))) / (2**(octave * -1))), now);
        }
    }

    function stopNote() {
        
        if (oscillator) {
                const now = audioContext.currentTime;
                const gain = gainNode.gain.value;
                gainNode.gain.cancelScheduledValues(now);
                gainNode.gain.value = gain;
                gainNode.gain.linearRampToValueAtTime(0, now + release);
                oscillator.stop(now + release);
                init = false;
            }
    }

    function onKeyDown(e: any) {
        if (e.key == keypress && !init) {
            playNote();
            keyHeld = true;
        }
    }

    function onKeyUp(e: any) {
        if (e.key == keypress) {
            stopNote();
            keyHeld = false;
        }
    }

    function mouseDown() {
        mousePressed = true;
        if (mouseEntered) {
            if (!keyHeld) {
                playNote()
            }
        }
    }

    function mouseUp() {
        mousePressed = false;
        if (!keyHeld) {
            stopNote();
        }
    }

    function mouseEnter() {
        mouseEntered = true;
        if (mousePressed && !keyHeld) {
            playNote()
        }
    }

    function mouseExit() {
        mouseEntered = false;
        if (!keyHeld) {
            stopNote()
        }
    }


    function writeNote() {
        let newNote: string;
        let flatSymbol = "♭";
        if (note.includes("s")) {
            newNote = note.replace("s", "#")
            newNote = newNote.replace("f", flatSymbol);
            newNote = newNote.replace(/\d+/g, '')
            const firstHalf = newNote.slice(0, 2);
            const secondHalf = newNote.slice(2);

            return `${firstHalf} ${secondHalf}`;
        }
        else {
            newNote = note;
            return newNote.replace(/\d+/g, '')
        }
        
    }

    let height = tweened(0, {
        duration: 1000, 
        easing: linear 
    });

    onMount(() => {
        bool = !bool;
        height.set(100);
    })
    
</script>


<svelte:window 
    on:keydown={onKeyDown}
    on:keyup|preventDefault={onKeyUp}
    on:mouseup={mouseUp}     
    on:mousedown={mouseDown} 
/>

    <!-- svelte-ignore a11y-no-static-element-interactions -->
    <!-- svelte-ignore a11y-mouse-events-have-key-events -->
    {#if bool}
	<div class=""
    style="height: {$height}%;"
    transition:fly={{ y: 1000, duration: 1000 }}>

	    <div  
            class="flex flex-col justify-end w-full h-full border-x-2 border-y-2 rounded-md items-center pb-12
            border-t-slate-200 border-r-slate-200 
            border-l-slate-500 border-b-slate-500

            {isBlackKey ? "bg-green-200" : "bg-purple-200 "}
            ${init ? "transition-transform scale-95 duration-100" : "transition-transform scale-100 duration-50"}"

            style="transform-origin: top; user-select: none;;"
            on:mouseenter={mouseEnter}  
            on:mouseleave={mouseExit}    
            

        >

            <div 
                style="user-select: none;"
                class="font-bold opacity-70 text-center p-2"
            >   {#if (noteOn)}
                    {writeNote()}
                {/if}
                {#if (controlOn)}
                    <strong>{keypress}</strong>
                {/if}
            </div>
                
        </div>
	</div>
    {/if}
        


    
    
