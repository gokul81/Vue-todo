<template>
  <div>
    <buttton
      @click="toggle = !toggle"
      class="px-3 py-3 text-white bg-yellow-700 w-20 mx-auto rounded-md mb-5 cursor-pointer"
      >Add Task</buttton
    >
    <form
      v-show="toggle"
      @submit.prevent="onSubmit"
      class="flex flex-col gap-5 border-2 p-2 mb-5 border-blue-700 bg-cyan-300"
    >
      <div class="flex flex-col">
        <label for="" class="mr-2 text-xl px-2 text-emerald-900">Name</label>
        <input
          v-model="name"
          type="text"
          name="title"
          class="border px-2 py-1 w-96"
        />
      </div>
      <button
        class="text-blue-700 text-xl bg-blue-700 rounded text-white ml-80"
        style="width: 100px"
        @click="toggle = !toggle"
      >
        Add
      </button>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      toggle: false,
      name: "",
    };
  },
  methods: {
    onSubmit(e) {
      this.$emit("onAdd", e);
    },
  },
};
</script>

<style></style>
