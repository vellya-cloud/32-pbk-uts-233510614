<script setup>
import { ref, computed } from 'vue';

const tasks = ref([]);
const newTask = ref('');
const filterType = ref('all');

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

const removeTask = (taskId) => {
  tasks.value = tasks.value.filter(task => task.id !== taskId);
}

const filterTasks = computed(() => {
  if (filterType.value === 'completed') {
    return tasks.value.filter(task => task.completed);
  } else if (filterType.value === 'active') {
    return tasks.value.filter(task => !task.completed);
  }
  return tasks.value;
})

</script>

<template>
  <input type="text" v-model="newTask" @keyup.enter="addTask" />
  <button @click="addTask">Add Task</button>
  <div>
    <button @click="filterType = 'all'">All</button>
    <button @click="filterType = 'completed'">Completed</button>
    <button @click="filterType = 'active'">Active</button>
  </div>

  <ul>
    <li v-for="task in filterTasks" :key="task.id">
      <input type="checkbox" v-model="task.completed" />
      {{ task.text }}
      <button @click="removeTask(task.id)">Remove</button>
    </li>
  </ul>
  <p>Tasks done: {{ taskCountDone }}</p>
</template>

<style scoped></style>
