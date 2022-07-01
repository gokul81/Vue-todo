<template>
  <div>
    <edit-button-vue
      v-show="task_edit_model"
      @onEdit="tasks_update"
      :item="task"
    ></edit-button-vue>

    <add-form @onAdd="addTask"></add-form>
    <div v-if="tasks.length > 0">
      <preview-vue>
        <div
          v-show="toggle"
          class="border-2 backdrop-blur-lg bg-cyan/400 text-blue-700 text-xl py-3 w-[40%] fixed top-[30%] right-[30%]"
        >
          <div>
            <div class="text-center">{{ task }}</div>
          </div>
          <div class="flex justify-end mx-3">
            <button
              @click="toggle = !toggle"
              class="bg-blue-700 text-white px-2 py-1 rounded"
            >
              OK
            </button>
          </div>
        </div>
      </preview-vue>

      <!-- <div v-if="isLoading" class="w-full text-center text-4xl text-blue-700">
        Loading...<br />Please wait
      </div> -->
      <task-item-vue
        :key="task.id"
        v-for="task in tasks"
        :item="task"
        @onShow="showTask"
        class="cursor-pointer"
      >
        <div class="flex flex-row gap-1">
          <div class="flex justify-end">
            <button
              @click="toggle = !toggle"
              class="text-base px-3 text-white py-1 bg-green-600 rounded-xl"
            >
              Show
            </button>
          </div>
          <div>
            <button
              @click="task_edit(task.id)"
              class="text-base px-4 text-white py-1 bg-blue-700 rounded-xl"
            >
              Edit
            </button>
          </div>
          <div>
            <button
              @click="tasks_remove(task.id)"
              class="text-base px-2 text-white py-1 bg-orange-700 rounded-xl"
            >
              Delete
            </button>
          </div>
        </div>
      </task-item-vue>
    </div>
    <div class="text-5xl text-red-700" v-else>No tasks</div>
  </div>
</template>

<script>
import TaskItemVue from "./TaskItem.vue";
import AddForm from "./AddForm.vue";
import PreviewVue from "./Preview.vue";
import EditButtonVue from "./EditButton.vue";
export default {
  data() {
    return {
      isLoading: false,
      tasks: [],
      task: [],
      editValue: null,
      toggle: false,
      task_preview_model: false,
      task_edit_model: false,
    };
  },
  methods: {
    async getTaskList() {
      this.isLoading = true;
      await fetch("http://localhost:8000/tasks")
        .then((res) => res.json())
        .then((json) => (this.tasks = json));
      // this.isLoading = false;
    },
    async showTask(taskid) {
      console.log(taskid);
      await fetch("http://localhost:8000/tasks/" + taskid)
        .then((res) => res.json())
        .then((json) => {
          this.task = json;
          console.log(json);
          return;
        });
    },
    async addTask(e) {
      console.log("Task add:", e);
      const data = new FormData(e.target);
      console.log(data);
      const user = Object.fromEntries(data.entries());
      const randomNum = Math.floor(Math.random() * 10000000);
      user.id = randomNum;
      const transformData = [...this.tasks, { ...user }];
      this.tasks = transformData;
      const wantedData = {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(user),
      };
      await fetch("http://localhost:8000/tasks/", wantedData).then((res) =>
        res.json()
      );
    },
    async tasks_remove(taskid) {
      console.log(taskid);
      const mutateData = this.tasks.filter((task) => task.id !== taskid);
      this.tasks = mutateData;
      await fetch("http://localhost:8000/tasks/" + taskid, { method: "DELETE" })
        .then((res) => res.json())
        .then((json) => console.log(json));
    },

    async task_edit(id) {
      this.task_edit_model = !this.task_edit_model;
    },
    async tasks_update(e, id, newTitle) {
      console.log(id);
      const data = new FormData(e.target);
      this.tasks.forEach((todo) => {
        if (todo.id === id) {
          todo.title = newTitle;
        }
      });

      const value = {
        title: newTitle,
      };
      // this.tasks = mutateData;
      this.task_edit_model = false;
      const requestOptions = {
        method: "PATCH",
        headers: {
          "Content-type": "application/json; charset=UTF-8",
        },
        body: JSON.stringify(value),
      };
      await fetch("http://localhost:8000/tasks/" + id, requestOptions).then(
        (res) => res.json()
      );
    },
  },
  components: {
    TaskItemVue,
    AddForm,
    EditButtonVue,
    PreviewVue,
  },
  mounted() {
    this.getTaskList();
  },
};
</script>

<style></style>
