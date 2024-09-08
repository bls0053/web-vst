<script lang="ts">


    export let text: string = "";
    export let size: number = 1;


</script>




<div 
style="width: calc(100% * {size / 6});"
class=" flex flex-col justify-between items-center h-full gap-1 pl-2 pr-2 border-4 select-none
border-l-green-400 border-b-green-400 
border-t-green-300 border-r-green-300 
bg-green-200 rounded-md">
    <strong>{text}</strong>
    <slot></slot>
</div>