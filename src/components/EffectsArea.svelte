<script lang="ts">

    import { Slider } from "$lib/components/ui/slider";
	import EffectCont from "./EffectCont.svelte";
    import { createEventDispatcher } from 'svelte';
	import EffectRow from "./EffectRow.svelte";
	import EffectLabel from "./EffectLabel.svelte";

	const dispatch = createEventDispatcher();
    let gainValue: number[] = [33];
    let adsrValue: number[][] = [[0.0],[0.1],[0.0],[0.0]]


    $: if (gainValue) {
        handleGain()
    }

    $: if (adsrValue) {
        handleAdsr()
    }

    function handleGain() {
        let gain = gainValue[0];
        dispatch('gainChange', {gain});
    }

    function handleAdsr() {
        let attack = adsrValue[0][0];
        let decay = adsrValue[1][0];
        let sustain = adsrValue[2][0];
        let release = adsrValue[3][0];

        let adsr = [attack, decay, sustain, release]
        dispatch('adsrChange', {adsr});
    }

 

</script>





<div class="flex flex-row w-full h-1/3 gap-3">

    <EffectCont text = "Gain Slider">
        <Slider bind:value={gainValue} max={33} step={1} class="max-w-[100%]" />
        {gainValue}
    </EffectCont>



    <EffectCont text = "ADSR">
        <EffectRow>
            <EffectLabel>Attack</EffectLabel>
            <Slider bind:value={adsrValue[0]} max={1} step={.01} class="max-w-[100%]" />
        </EffectRow>
        <EffectRow>
            <EffectLabel>Decay</EffectLabel>
            <Slider bind:value={adsrValue[1]} max={1} step={.01} class="max-w-[100%]" />
        </EffectRow>
        <EffectRow>
            <EffectLabel>Sustain</EffectLabel>
            <Slider bind:value={adsrValue[2]} max={1} step={.01} class="max-w-[100%]" />
        </EffectRow>
        <EffectRow>
            <EffectLabel>Release</EffectLabel>
            <Slider bind:value={adsrValue[3]} max={1} step={.01} class="max-w-[100%]" />
        </EffectRow>

        
        
    </EffectCont>

    <EffectCont text = "effect">

    </EffectCont>

    <EffectCont text = "effect">

    </EffectCont>
    
    
</div>