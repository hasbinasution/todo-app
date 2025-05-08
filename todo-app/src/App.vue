<script setup>
import { ref, computed } from 'vue';

const newTask = ref('');
const tasks = ref([]);
const errorMessage = ref('');
const showOnlyIncomplete = ref(false);

// Tambah kegiatan
function addTask() {
  if (newTask.value.trim()) {
    tasks.value.push({ text: newTask.value, completed: false });
    newTask.value = '';
    errorMessage.value = '';
  } else {
    errorMessage.value = 'Kegiatan tidak boleh kosong!';
  }
}

// Hapus kegiatan
function deleteTask(index) {
  tasks.value.splice(index, 1);
}

// Checklist selesai
function toggleTask(index) {
  tasks.value[index].completed = !tasks.value[index].completed;
}

// Filter kegiatan belum selesai
const filteredTasks = computed(() =>
  showOnlyIncomplete.value
    ? tasks.value.filter(task => !task.completed)
    : tasks.value
);
</script>

<template>
  <div class="container">
    <h1>🌟 To-Do List</h1>

    <input
      v-model="newTask"
      @keyup.enter="addTask"
      placeholder="Tambahkan kegiatan"
      type="text"
    />
    <button @click="addTask">Tambah</button>

    <p v-if="errorMessage" class="error">{{ errorMessage }}</p>

    <!-- Filter -->
    <div class="filter">
      <label>
        <input type="checkbox" v-model="showOnlyIncomplete" />
        Tampilkan hanya kegiatan yang belum selesai
      </label>
    </div>

    <!-- Daftar kegiatan dengan animasi -->
    <transition-group name="list" tag="ul" v-if="filteredTasks.length > 0">
      <li v-for="(task, index) in filteredTasks" :key="task.text" class="task-item">
        <div class="task-content">
          <input type="checkbox" v-model="task.completed" @change="toggleTask(index)" />
          <span :class="{ completed: task.completed }">{{ task.text }}</span>
        </div>
        <button class="delete-button" @click="deleteTask(index)">Hapus</button>
      </li>
    </transition-group>
    <p v-else>Tidak ada kegiatan yang ditampilkan.</p>
  </div>
</template>

<style scoped>
/* Struktur dasar */
.container {
  max-width: 600px;
  margin: 40px auto;
  padding: 30px;
  background: linear-gradient(to right, #fdfbfb, #ebedee);
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
  border-radius: 12px;
  font-family: 'Segoe UI', sans-serif;
  text-align: center;
}

/* Input */
input[type="text"] {
  padding: 10px;
  width: 65%;
  margin-right: 10px;
  border: 1px solid #ccc;
  border-radius: 8px;
  outline: none;
}

/* Tombol tambah & hapus */
button {
  padding: 10px 16px;
  border: none;
  border-radius: 8px;
  background-color: #007bff;
  color: white;
  cursor: pointer;
}
button:hover {
  background-color: #0056b3;
}

/* Daftar */
ul {
  list-style: none;
  padding: 0;
  margin-top: 20px;
}
li.task-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: #ffffff;
  margin-bottom: 12px;
  padding: 12px 16px;
  border-radius: 10px;
  box-shadow: 0 3px 6px rgba(0, 0, 0, 0.05);
}

/* Konten tugas */
.task-content {
  display: flex;
  align-items: center;
  gap: 12px;
}

/* Tugas selesai */
.completed {
  text-decoration: line-through;
  color: #888;
  font-style: italic;
}

/* Tombol hapus */
.delete-button {
  background-color: #ff4d4d;
}
.delete-button:hover {
  background-color: #e60000;
}

/* Error */
.error {
  color: red;
  margin-top: 10px;
}

/* Filter */
.filter {
  margin-top: 20px;
  font-size: 0.9rem;
}

/* Transisi animasi */
.list-enter-active, .list-leave-active {
  transition: all 0.3s ease;
}
.list-enter-from {
  opacity: 0;
  transform: translateY(10px);
}
.list-leave-to {
  opacity: 0;
  transform: translateY(10px);
}
</style>
