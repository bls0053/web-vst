<script lang="ts">
	import EffectsArea from "./EffectsArea.svelte";
    import HeaderArea from "./HeaderArea.svelte";
	import KeyArea from "./KeyArea.svelte";



    let gainValue: number;
    let adsrValue: number[];

    function setGain(event: any) {
        let newGain = event.detail.gain; 
        gainValue = newGain;
        console.log(gainValue)
    }

    function setAdsr(event: any) {
        let newAdsr = event.detail.adsr; 
        adsrValue = newAdsr;
        console.log(adsrValue)
    }

</script>



<div class="flex flex-col gap-2 w-full h-full p-2 border-8
border-l-orange-400 border-b-orange-400 
border-t-orange-300 border-r-orange-300 
bg-orange-200 rounded-md">

    
    <HeaderArea/>
    <EffectsArea 
        on:gainChange={setGain}
        on:adsrChange={setAdsr}
    />
    <KeyArea 
    gainValue={gainValue}
    adsrValue={adsrValue}
    />


</div>