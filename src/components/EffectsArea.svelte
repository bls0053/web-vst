<script lang="ts">

    import Icon from '@iconify/svelte';

	import EffectCont from "./EffectCont.svelte";
    import { createEventDispatcher, onMount } from 'svelte';
	import EffectRow from "./EffectRow.svelte";
	import EffectLabel from "./EffectLabel.svelte";
	import EffectKnob from './EffectKnob.svelte';
	import EffectCol from './EffectCol.svelte';
	import WaveCont from './WaveCont.svelte';
	import FilterCont from './FilterCont.svelte';

	const dispatch = createEventDispatcher();

    let adsrValue: number[] = [0.1, 0.1, 1, 0.1];
    let filterFields: number[] = [10000, 500, 0, 100];
    let currWave: string = "square";
    let currFilter: string = "allpass";


    $: if (adsrValue) {
        dispatchAdsr();
    }

    $: if (filterFields) {
        dispatchFilterFields();
    }

    $: if (currWave) {
        dispatchWave();
    }

    $: if (currFilter) {
        dispatchFilter();
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

    function dispatchFilter() {
        let filter = currFilter;
        dispatch('effectAreaFilter', {filter})
    }

    function dispatchFilterFields() {
        let filter = currFilter;
        dispatch('effectAreaFilterFields', {filterFields})
    }


    onMount(() => {
        dispatchAdsr();
        dispatchWave();
        dispatchFilter();
        dispatchFilterFields();
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
                <Icon style="font-size: 40px; color: rgb(147 51 234 / var(--tw-bg-opacity));" icon="ph:wave-sine-bold"/>
            </WaveCont>
            <WaveCont id="square" bind:currWave={currWave}>
                <Icon style="font-size: 40px; color: rgb(147 51 234 / var(--tw-bg-opacity));" icon="ph:wave-square-bold"/>
            </WaveCont>
            
        </EffectRow>
        <EffectRow>

            <WaveCont id="sawtooth" bind:currWave={currWave}>
                <Icon style="font-size: 40px; color: rgb(147 51 234 / var(--tw-bg-opacity));" icon="ph:wave-sawtooth-bold"/>
            </WaveCont>
            <WaveCont id="triangle" bind:currWave={currWave}>
                <Icon style="font-size: 40px; color: rgb(147 51 234 / var(--tw-bg-opacity));" icon="ph:wave-triangle-bold"/>
            </WaveCont>
            
        </EffectRow>
        </EffectCol>
             
        
        
    </EffectCont>


    
    <EffectCont size={3}>
    
        <EffectRow>
            <EffectCol size={3}>
                <EffectLabel>Filter</EffectLabel>
                <EffectRow>
                    <FilterCont id="lowpass" bind:currFilter={currFilter}>
                        <Icon style="font-size: 40px; color: rgb(147 51 234 / var(--tw-bg-opacity));" icon="fad:filter-lowpass"/>
                    </FilterCont>
                    <FilterCont id="highpass" bind:currFilter={currFilter}>
                        <Icon style="font-size: 40px; color: rgb(147 51 234 / var(--tw-bg-opacity));" icon="fad:filter-highpass"/>
                    </FilterCont>
                    <FilterCont id="bandpass" bind:currFilter={currFilter}>
                        <Icon style="font-size: 40px; color: rgb(147 51 234 / var(--tw-bg-opacity));" icon="fad:filter-bandpass"/>
                    </FilterCont>
                    <FilterCont id="notch" bind:currFilter={currFilter}>
                        <Icon style="font-size: 40px; color: rgb(147 51 234 / var(--tw-bg-opacity));" icon="fad:filter-notch"/>
                    </FilterCont>
                </EffectRow>

                <EffectRow>
                    <FilterCont id="lowshelf" bind:currFilter={currFilter}>
                        <Icon style="font-size: 40px; color: rgb(147 51 234 / var(--tw-bg-opacity));" icon="fad:filter-shelving-lo"/>
                    </FilterCont>
                    <FilterCont id="highshelf" bind:currFilter={currFilter}>
                        <Icon style="font-size: 40px; color: rgb(147 51 234 / var(--tw-bg-opacity));" icon="fad:filter-shelving-hi"/>
                    </FilterCont>
                    <FilterCont id="peaking" bind:currFilter={currFilter}>
                        <Icon style="font-size: 40px; color: rgb(147 51 234 / var(--tw-bg-opacity));" icon="fad:filter-bell"/>
                    </FilterCont>
                    <FilterCont id="allpass" bind:currFilter={currFilter}>
                        <Icon style="font-size: 40px; color: rgb(147 51 234 / var(--tw-bg-opacity));" icon="fad:filter-bypass"/>
                    </FilterCont>



                </EffectRow>
            </EffectCol>
            <EffectCol size={3}>
                <EffectRow size={3}>
                    <EffectKnob textSize={40} bind:value={filterFields[0]} min={20} max={20000}></EffectKnob>
                    <EffectLabel width={3} size="small">Frequency</EffectLabel>
                        
                </EffectRow>
                <EffectRow size={3}>
                    <EffectKnob textSize={40} bind:value={filterFields[1]} min={0.0001} max={1000}></EffectKnob>
                    <EffectLabel width={3} size="small">Bandwidth</EffectLabel>
                        
                </EffectRow>
                <EffectRow size={3}>
                    <EffectKnob textSize={40} bind:value={filterFields[2]} min={-40} max={40}></EffectKnob>
                    <EffectLabel width={3} size="small">Gain</EffectLabel>
                    
            </EffectRow>
                <EffectRow size={3}>
                    <EffectKnob textSize={40} bind:value={filterFields[3]} min={0} max={100}></EffectKnob>
                    <EffectLabel width={3} size="small">Mix</EffectLabel>
                </EffectRow>
            </EffectCol>
        </EffectRow>
        
    </EffectCont>



    
    
    
</div>


<!-- <EffectRow>
                <FilterCont id="allpass" bind:currFilter={currFilter}>
                    <Icon style="font-size: 50px; color: rgb(147 51 234 / var(--tw-bg-opacity));" icon="fad:filter-bypass"/>
                </FilterCont>
            </EffectRow> -->