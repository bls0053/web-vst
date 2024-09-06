<script lang="ts">
	import Key from "./Key.svelte";
    import { afterUpdate, onMount } from "svelte";
    import notes from "./KeyData.json";

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

    let notesData:NotesData = notes;
    let audioContext: AudioContext;

    let masterGain: GainNode;
    let init: boolean = false;

    let container: HTMLDivElement;
    let observer: MutationObserver;


    $: if (masterGain) {
        masterGain.gain.value = gainValue / 100;
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


<div bind:this={container} class="grid-container w-full h-full ">
    {#if init}
        {#each Object.keys(notesData) as note}
            <div
            class={notesData[note].isBlackKey ? 'black-key' : 'white-key'}>
                <Key 
                note={note}
                adsr={adsrValue}
                keypress={notesData[note].keypress}
                frequency={notesData[note].frequency}
                audioContext={audioContext}
                masterGain={masterGain}
                isBlackKey={notesData[note].isBlackKey}/>
            </div>
        {/each}
    {/if}
    

    
    
</div>
<button on:click={start}>start</button>