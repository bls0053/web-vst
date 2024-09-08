<script lang="ts">
	import EffectCol from "./EffectCol.svelte";
    import EffectCont from "./EffectCont.svelte";
	import EffectKnob from "./EffectKnob.svelte";
	import EffectLabel from "./EffectLabel.svelte";
	import EffectRow from "./EffectRow.svelte";
    import EffectsArea from "./EffectsArea.svelte";
    import KeyBoard from "./KeyBoard.svelte";

    export let gainValue: number = 33;
    export let adsrValue: number[];
    export let waveValue: string;



</script>



<div class="flex flex-row justify-center w-full h-1/2 gap-3">

    
    <KeyBoard
        adsrValue={adsrValue}
        gainValue={gainValue}
        waveValue={waveValue}
    />

    <EffectCont>
        <EffectRow>
            <EffectCol>
                <EffectLabel>Gain</EffectLabel>
                <EffectKnob bind:value={gainValue} min={0} max={100}></EffectKnob>
            </EffectCol>
        </EffectRow>

        <EffectRow>

            
        </EffectRow>

        <EffectRow>

            
        </EffectRow>
        <EffectRow>

            
        </EffectRow>
        


    </EffectCont>


    
</div>