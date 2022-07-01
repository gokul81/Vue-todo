<template>
  <div class="flex justify-center">
    <form
      @submit.prevent="onSubmit($event, item.id)"
      class="mb-2 bg-blue-700  p-5 fixed top-[30%]"
    >
      <div class="mb-2 flex flex-col">
        <label class="text-white">Name:</label>
        <input type="text" name="name" v-model="item.title" style="width:300px" />
      </div>
      <div class="flex justify-center">
        <button @click="toggle = !toggle" class="rounded-md bg-white px-2">
          Save
        </button>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  props: ["show", "item"],
 
  methods: {
    onSubmit(e, id) {
      console.log(e, this.item.title);
      this.$emit("onEdit", e, id, this.item.title);
    },
  },
 
};
</script>

<style></style>
