<script lang="ts">

    import Icon from '@iconify/svelte';

	import EffectCont from "./EffectCont.svelte";
    import { createEventDispatcher, onMount } from 'svelte';
	import EffectRow from "./EffectRow.svelte";
	import EffectLabel from "./EffectLabel.svelte";
	import EffectKnob from './EffectKnob.svelte';
	import EffectCol from './EffectCol.svelte';
	import WaveCont from './WaveCont.svelte';

	const dispatch = createEventDispatcher();

    let adsrValue: number[] = [0.1, 0.1, 1, 0.1]
    let currWave: string = "square"


    $: if (adsrValue) {
        dispatchAdsr();
    }

    $: if (currWave) {
        dispatchWave();
    }

    function dispatchAdsr() {
        let adsr = adsrValue;
        // console.log("effect-", adsr)
        dispatch('effectAreaAdsr', {adsr});
    }

    function dispatchWave() {
        let wave = currWave;
        // console.log("effect-", wave)
        dispatch('effectAreaWave', {wave});
    }

    onMount(() => {
        dispatchAdsr();
        dispatchWave();
    })
 

</script>





<div class="flex flex-row w-full h-1/3 gap-3">


    <EffectCont size={2}>
        <EffectRow>
            <EffectCol>
                <EffectLabel>Attack</EffectLabel>
                <EffectKnob bind:value={adsrValue[0]} min={0} max={5}></EffectKnob>
            </EffectCol>
            <EffectCol>
                <EffectLabel>Decay</EffectLabel>
                <EffectKnob bind:value={adsrValue[1]} min={0} max={5}></EffectKnob>
            </EffectCol>
        </EffectRow>
        <EffectRow>
            <EffectCol>
                <EffectLabel>Sustain</EffectLabel>
                <EffectKnob bind:value={adsrValue[2]} min={0} max={1}></EffectKnob>
            </EffectCol>
            <EffectCol>
                <EffectLabel>Release</EffectLabel>
                <EffectKnob bind:value={adsrValue[3]} min={0} max={5}></EffectKnob>
            </EffectCol>
        </EffectRow>
    </EffectCont>

 
    
    <EffectCont>
        <EffectCol>
            <EffectLabel>Wave</EffectLabel>
        <EffectRow>

            <WaveCont id="sine" bind:currWave={currWave}>
                <Icon style="font-size: 50px; color: rgb(147 51 234 / var(--tw-bg-opacity));" icon="ph:wave-sine-bold"/>
            </WaveCont>
            <WaveCont id="square" bind:currWave={currWave}>
                <Icon style="font-size: 50px; color: rgb(147 51 234 / var(--tw-bg-opacity));" icon="ph:wave-square-bold"/>
            </WaveCont>
            
        </EffectRow>
        <EffectRow>

            <WaveCont id="sawtooth" bind:currWave={currWave}>
                <Icon style="font-size: 50px; color: rgb(147 51 234 / var(--tw-bg-opacity));" icon="ph:wave-sawtooth-bold"/>
            </WaveCont>
            <WaveCont id="triangle" bind:currWave={currWave}>
                <Icon style="font-size: 50px; color: rgb(147 51 234 / var(--tw-bg-opacity));" icon="ph:wave-triangle-bold"/>
            </WaveCont>
            
        </EffectRow>
        </EffectCol>
             
        
        
    </EffectCont>


    
    <EffectCont><EffectRow>
        <EffectCol>
            <EffectLabel>coming soon :D</EffectLabel>

        </EffectCol>
        
    </EffectRow></EffectCont>
    <EffectCont size={2}><EffectRow>
        <EffectCol>
            <EffectLabel>coming soon :D</EffectLabel>

        </EffectCol>
        
    </EffectRow></EffectCont>
  
    
    
</div>