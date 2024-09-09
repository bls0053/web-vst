<script lang="ts">
	import Key from "./Key.svelte";
    import { afterUpdate, createEventDispatcher, onMount } from "svelte";
    import notes from "./KeyDataC2.json";
	import WaveCont from "./WaveCont.svelte";

    interface Note {
        keypress: string;
        frequency: number;
        isBlackKey: boolean;
    }
    type NotesData = {
        [key: string]: Note;
    }


    export let gainValue: number;
    export let adsrValue: number[];
    export let waveValue: string;
    export let octaveValue: number;
    export let controlOn: boolean;
    export let noteOn: boolean;
    export let pitchValue: number;
    export let filterValue: string;
    export let filterFields: number[];
    export let masterGain: GainNode;
    export let audioContext: AudioContext;
    
    const dispatch = createEventDispatcher()

    let notesData:NotesData = notes;
    

    
    let init: boolean = false;

    let container: HTMLDivElement;
    let observer: MutationObserver;
   

    $: if (audioContext) {
        dispatch("acontx", {audioContext})
    }

    $: if (masterGain) {
        masterGain.gain.value = gainValue / 300;
        dispatch("master", {masterGain})
    }

    function start() {
        audioContext = new AudioContext();
        masterGain = audioContext.createGain();
        masterGain.gain.value = .33;
        masterGain.connect(audioContext.destination);
        init = true;

    }

    function positionBlackKeys() {
        let blackKeys = document.querySelectorAll('.black-key');
        blackKeys.forEach(key => {
            const prevKey = key.previousElementSibling as HTMLElement;
            const curr = key as HTMLElement;
            if (prevKey) {
            const prevKeyWidth = prevKey.offsetWidth;
            const prevKeyHeight = prevKey.offsetHeight;
            const prevKeyOffsetLeft = prevKey.offsetLeft;
            
            const keyWidth = prevKeyWidth * (2/3);
            const keyHeight = prevKeyHeight * (3/5);
            const keyOffsetLeft = prevKeyOffsetLeft + prevKeyWidth - (.5 * keyWidth);
            

            curr.style.position = 'absolute';
            curr.style.left = `${keyOffsetLeft}px`;
            curr.style.height = `${keyHeight}px`;
            curr.style.width = `${keyWidth}px`;

            // console.log(prevKeyWidth, " ", keyWidth)
            // console.log(prevKeyHeight, " ", keyHeight)
            // console.log(prevKeyOffsetLeft, " ", keyOffsetLeft)
            }
        });
    }

    onMount(() => {
        observer = new MutationObserver(() => {
            positionBlackKeys();
        });

        if (container) {
            observer.observe(container, { childList: true, subtree: true});
        }
    });

    $: if (init) {
        const timer = setTimeout(() => {
            positionBlackKeys();
        }, 1300);
    }


</script>

<style>
    .grid-container {
        display: grid;
        grid-template-columns: repeat(21, 1fr);
    }

    .black-key {
        z-index: 10;
        position: absolute;
    }

    .white-key {
        height: 100%;
        width: 100%;
    }

</style>


<svelte:window 
    on:resize={positionBlackKeys}
/>

<div class="flex flex-col justify-center w-5/6">
    <div bind:this={container} class="grid-container w-full h-full">
        {#if init}
            {#each Object.keys(notesData) as note}
                <div
                class=" 
                {notesData[note].isBlackKey ? 'black-key' : 'white-key'}">
                    <Key 
                    note={note}
                    adsr={adsrValue}
                    wave={waveValue}
                    octave={octaveValue}
                    noteOn={noteOn}
                    controlOn={controlOn}
                    pitch={pitchValue}
                    filter={filterValue}
                    filterFields={filterFields}

                    keypress={notesData[note].keypress}
                    frequency={notesData[note].frequency}
                    audioContext={audioContext}
                    masterGain={masterGain}
                    isBlackKey={notesData[note].isBlackKey}/>
                </div>
            {/each}
        {/if}
    </div>
    {#if !init}
    <div class="flex flex-row h-full justify-center items-center">
        <button class="w-1/2 bg-purple-200 text-2xl rounded-full p-4 font-bold" on:click={start}>Start</button>
    </div>
        
    {/if}

</div>