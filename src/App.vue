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
  <div class="min-h-screen bg-gradient-to-br from-indigo-100 to-yellow-50 flex items-center justify-center p-6">
    <div class="bg-white rounded-3xl shadow-2xl w-full max-w-lg p-6 space-y-6">

      <h1 class="text-center text-3xl font-bold text-indigo-600">Daftar Tugas</h1>

      <div class="flex items-center bg-indigo-50 rounded-full px-4 py-2 shadow-inner">
        <input
          v-model="newTask"
          @keyup.enter="addTask"
          type="text"
          placeholder="Tambahkan tugas baru..."
          class="flex-1 bg-transparent outline-none text-indigo-700 placeholder-indigo-300"
        />
        <button
          @click="addTask"
          class="ml-3 bg-yellow-400 text-indigo-900 font-semibold px-4 py-1.5 rounded-full hover:bg-yellow-300 transition"
        >
          +
        </button>
      </div>

      <div class="flex justify-center gap-3">
        <span
          v-for="type in ['all', 'completed', 'active']"
          :key="type"
          @click="filterType = type"
          :class="filterType === type ? 'bg-indigo-500 text-white' : 'bg-indigo-100 text-indigo-600'"
          class="px-4 py-1.5 rounded-full text-sm cursor-pointer font-medium transition hover:bg-indigo-200"
        >
          {{ type.charAt(0).toUpperCase() + type.slice(1) }}
        </span>
      </div>

      <ul class="space-y-4 max-h-64 overflow-y-auto pr-2 scroll-smooth">
        <li
          v-for="task in filterTasks"
          :key="task.id"
          class="flex justify-between items-center bg-white border border-indigo-100 rounded-xl px-4 py-3 shadow-sm hover:shadow-md transition"
        >
          <div class="flex items-center gap-3">
            <input type="checkbox" v-model="task.completed" class="accent-indigo-500 w-5 h-5" />
            <span :class="task.completed ? 'line-through text-gray-400' : 'text-gray-700'">
              {{ task.text }}
            </span>
          </div>
          <button
            @click="removeTask(task.id)"
            class="text-pink-500 hover:text-pink-700 font-bold text-lg"
          >
            ✕
          </button>
        </li>
      </ul>

      <div class="text-center text-indigo-700 text-md font-medium">
        Selesai: {{ taskCountDone }} tugas
      </div>
    </div>
  </div>
</template>

<style scoped></style>
