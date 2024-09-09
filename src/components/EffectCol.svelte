<script lang="ts">

    export let size: number = 6;

</script>




<div 
style="width: calc(100% * {size / 6});"
class="flex flex-col justify-between items-center w-full h-full gap-1">
    <slot></slot>
</div>