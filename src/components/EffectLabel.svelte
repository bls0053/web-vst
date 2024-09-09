<script lang="ts">

    export let size: string = "medium";
    export let width: number = 6;

</script>




<div 
style="font-size: {size}; width: calc(100% * {width / 6});"
class="flex flex-row justify-center items-center font-bold w-full bg-purple-200 rounded-md select-none">
    <slot></slot>
</div>