<script>
import ToDoheadVue from './components/ToDohead.vue';
import ToDobodyVue from './components/ToDobody.vue';
import TaskListVue from './components/TaskList.vue';
export default{
components:{
  ToDoheadVue,
  ToDobodyVue,
  TaskListVue
}
}
</script>

<template>
<ToDoheadVue></ToDoheadVue>
<ToDobodyVue>
  <!-- <h2 class="text-2xl mb-8 text-blue-700" style="text-decoration: underline;">Tasks To-Do</h2> -->
  <task-list-vue></task-list-vue>
</ToDobodyVue>

</template>
