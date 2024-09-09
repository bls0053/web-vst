<script lang="ts">
	import EffectsArea from "./EffectsArea.svelte";
    import HeaderArea from "./HeaderArea.svelte";
	import KeyArea from "./KeyArea.svelte";



    // let gainValue: number;
    let adsrValue: number[];
    let waveValue: string;
    let filterValue: string;
    let filterFields: number[];

    let audioContext: AudioContext;
    let masterGain: GainNode;


    function setAdsr(event: any) {
        adsrValue = event.detail.adsr;
        console.log(adsrValue)
    }

    function setWave(event: any) {
        waveValue = event.detail.wave;
        console.log(waveValue)
    }

    function setFilter(event: any) {
        filterValue = event.detail.filter;
        console.log(filterValue)
    }

    function setFilterFields(event: any) {
        filterFields = event.detail.filterFields;
        console.log(filterFields)
    }

</script>



<div class="flex flex-col gap-2 w-full h-full p-2 border-8
border-l-orange-400 border-b-orange-400 
border-t-orange-300 border-r-orange-300 
bg-orange-200 rounded-md">

    
    <HeaderArea/>
    <EffectsArea
        on:effectAreaAdsr={setAdsr}
        on:effectAreaWave={setWave}
        on:effectAreaFilter={setFilter}
        on:effectAreaFilterFields={setFilterFields}
        masterGain={masterGain}
        audioContext={audioContext}
    />
    <KeyArea 
    adsrValue={adsrValue}
    waveValue={waveValue}
    filterValue={filterValue}
    filterFields={filterFields}
    bind:masterGain={masterGain}
    bind:audioContext={audioContext}
    />


</div>