<script lang="ts">


    export let text;
    export let size: number = 1;


</script>




<div 
style="width: calc(100% * {size / 6});"
class=" flex flex-col justify-center items-center h-full p-6 gap-1 border-4
border-l-green-400 border-b-green-400 
border-t-green-300 border-r-green-300 
bg-green-200 rounded-md">
    <strong>{text}</strong>
    <slot></slot>
</div>