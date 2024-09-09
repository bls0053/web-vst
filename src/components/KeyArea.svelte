<script lang="ts">

    import EffectCol from "./EffectCol.svelte";
    import EffectCont from "./EffectCont.svelte";
	import EffectKnob from "./EffectKnob.svelte";
	import EffectLabel from "./EffectLabel.svelte";
	import EffectRow from "./EffectRow.svelte";
    import EffectsArea from "./EffectsArea.svelte";
    import KeyBoard from "./KeyBoard.svelte";
    import OctaveEffect from "./OctaveEffect.svelte";
    import ModeToggles from "./ModeToggles.svelte";
	import Slider from "./Slider.svelte";


    export let gainValue: number = 33;
    export let adsrValue: number[];
    export let waveValue: string;
    export let filterValue: string;
    export let filterFields: number[];
    
    let controlOn: boolean = false;
    let noteOn: boolean = false;
    let octaveValue: number = 0;
    let pitch: number = 2;
    




</script>



<div class="flex flex-row justify-center w-full h-1/2 gap-3">



    <KeyBoard
        adsrValue={adsrValue}
        gainValue={gainValue}
        waveValue={waveValue}
        octaveValue={octaveValue}
        noteOn={noteOn}
        controlOn={controlOn}
        pitchValue={pitch}
        filterValue={filterValue}
        filterFields={filterFields}
    />

    <EffectCont>
        <EffectRow size={4}>
            <EffectCol>
                <EffectLabel size="small">Volume</EffectLabel>
                <EffectRow>
                    <EffectKnob bind:value={gainValue} min={0} max={100} textSize={50}></EffectKnob>
                </EffectRow>
            </EffectCol>
        </EffectRow>

        <EffectRow size={4}>
            <EffectCol size={3}>
                <EffectLabel size="small">Octave</EffectLabel>
                <OctaveEffect bind:currOctave={octaveValue}></OctaveEffect>
            </EffectCol>
            <EffectCol size={3}>
                <EffectLabel size="small">Modes</EffectLabel>
                <ModeToggles bind:noteOn={noteOn} bind:controlOn={controlOn}></ModeToggles>
            </EffectCol>
        </EffectRow>

        <EffectRow size={4}>
            <EffectCol>
                <EffectLabel size="small">Pitch Bend</EffectLabel>
                <EffectRow>
                    <Slider bind:value={pitch} max={4} step={.01} />
                    <div class="w-1/5">{(pitch - 2).toFixed(2)}</div>
                    
                </EffectRow>
            </EffectCol>
        </EffectRow>
    </EffectCont>


    
</div>