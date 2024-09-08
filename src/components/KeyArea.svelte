<script lang="ts">
	import Slider from "$lib/components/ui/slider/slider.svelte";
    import EffectCol from "./EffectCol.svelte";
    import EffectCont from "./EffectCont.svelte";
	import EffectKnob from "./EffectKnob.svelte";
	import EffectLabel from "./EffectLabel.svelte";
	import EffectRow from "./EffectRow.svelte";
    import EffectsArea from "./EffectsArea.svelte";
    import KeyBoard from "./KeyBoard.svelte";
    import OctaveEffect from "./OctaveEffect.svelte";
    import ModeToggles from "./ModeToggles.svelte";

    export let gainValue: number = 33;
    export let adsrValue: number[];
    export let waveValue: string;

    let controlOn: boolean = false;
    let noteOn: boolean = false;
    let octaveValue: number = 0;


</script>



<div class="flex flex-row justify-center w-full h-1/2 gap-3">



    <KeyBoard
        adsrValue={adsrValue}
        gainValue={gainValue}
        waveValue={waveValue}
        octaveValue={octaveValue}
        noteOn={noteOn}
        controlOn={controlOn}
    />

    <EffectCont>
        <EffectRow>
            <EffectCol>
                <EffectLabel>Gain</EffectLabel>
                <EffectKnob bind:value={gainValue} min={0} max={100}></EffectKnob>
            </EffectCol>
        </EffectRow>

        <EffectRow>
            <EffectCol>
                <EffectLabel size="small">Octave</EffectLabel>
                <OctaveEffect bind:currOctave={octaveValue}></OctaveEffect>
            </EffectCol>
            <EffectCol>
                <EffectLabel size="small">Modes</EffectLabel>
                <ModeToggles bind:noteOn={noteOn} bind:controlOn={controlOn}></ModeToggles>
            </EffectCol>


        </EffectRow>

        <EffectRow>
            <EffectCol>
                <EffectLabel>coming soon :/</EffectLabel>
                <Slider/>
            </EffectCol>
            
            
        </EffectRow>
        <EffectRow>
            <EffectCol>
                <EffectLabel>coming soon :/</EffectLabel>
                <Slider/>
            </EffectCol>
            
        </EffectRow>
        


    </EffectCont>


    
</div>