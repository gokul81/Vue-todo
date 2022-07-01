<script>
export default{
props:['item'],
emits:['onDelete','onShow','onEdit']

}

</script>

<template>
<div class="p-2 rounded mb-4 bg-white flex justify-between border-blue-700 hover:border-t-8 border-2 shadow-2xl mt-4 text-blue-700 text-xl ">
    {{item.title}}
    <div class="flex row-reverse" style="gap:1rem">
    <div  @click="this.$emit('onShow', item.id)" class="text-blue-700 text-xl" >
      <slot></slot>
    </div>

        
       
    </div>
     
    </div>
</template>

<style>
</style>