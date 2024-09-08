<script lang="ts">

    export let size: string = "medium";

</script>




<div 
style="font-size: {size};"
class="flex flex-row justify-center items-center font-bold w-full bg-purple-200 rounded-md select-none">
    <slot></slot>
</div>