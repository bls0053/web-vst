<script lang="ts">
	import KeyBoard from "./KeyBoard.svelte";

    export let gainValue: number;
    export let adsrValue: number[];



</script>



<div class="flex flex-col justify-center w-full h-1/2 ">

    
    <KeyBoard
    adsrValue={adsrValue}
    gainValue={gainValue}
    />


    
</div>