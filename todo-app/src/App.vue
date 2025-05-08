<script setup>
import { ref } from 'vue';

const newTask = ref('');
const tasks = ref([]);

// Fungsi untuk menambahkan kegiatan ke daftar
function addTask() {
  if (newTask.value.trim()) {
    tasks.value.push({ text: newTask.value, completed: false });
    newTask.value = '';
  }
}
</script>

<template>
  <div class="container">
    <h1>To-Do List</h1>

    <!-- Input untuk kegiatan baru -->
    <input
      v-model="newTask"
      @keyup.enter="addTask"
      placeholder="Tambahkan kegiatan"
    />
    <button @click="addTask">Tambah</button>

    <!-- Menampilkan daftar kegiatan -->
    <ul>
      <li v-for="(task, index) in tasks" :key="index">
        {{ task.text }}
      </li>
    </ul>
  </div>
</template>

<style scoped>
.container {
  max-width: 500px;
  margin: 0 auto;
  padding: 20px;
  font-family: sans-serif;
  text-align: center;
}
input {
  padding: 8px;
  width: 70%;
  margin-right: 10px;
}
button {
  padding: 8px 12px;
}
ul {
  list-style: none;
  padding: 0;
  margin-top: 20px;
}
li {
  text-align: left;
  margin-bottom: 10px;
  padding: 8px;
  background: #f2f2f2;
  border-radius: 5px;
}
</style>
