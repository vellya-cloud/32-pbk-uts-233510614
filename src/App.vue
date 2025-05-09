<script setup>
import { ref, computed } from 'vue';

const tasks = ref([]);
const newTask = ref('');

const addTask = () => {
  if (newTask.value.trim() !== '') {
    tasks.value.push(
      {
        id: tasks.value.length + 1,
        text: newTask.value,
        completed: false,
      });
    newTask.value = '';
  }
};

const taskCountDone = computed(() => {
  return tasks.value.filter(task => task.completed).length;
})

</script>

<template>
  <input type="text" v-model="newTask" @keyup.enter="addTask" />
  <button @click="addTask">Add Task</button>

  <ul>
    <li v-for="task in tasks" :key="task.id">
      <input type="checkbox" v-model="task.completed" />
      {{ task.text }}
    </li>
  </ul>
  <p>Tasks done: {{ taskCountDone }}</p>
</template>

<style scoped></style>
