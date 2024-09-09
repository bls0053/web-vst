<script lang="ts">

    export let size: number = 12;

</script>




<div 
style="height: calc(100% * {size / 12});"
class=" flex flex-row justify-evenly items-center w-full h-full gap-2 pb-1">
    <slot></slot>
</div>