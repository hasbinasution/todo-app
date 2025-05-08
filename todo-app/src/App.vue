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

// Toggle selesai
function toggleTask(index) {
  tasks.value[index].completed = !tasks.value[index].completed;
}

// Filter: hanya kegiatan yang belum selesai
const filteredTasks = computed(() =>
  showOnlyIncomplete.value
    ? tasks.value.filter(task => !task.completed)
    : tasks.value
);
</script>

<template>
  <div class="container">
    <h1>To-Do List</h1>

    <input
      v-model="newTask"
      @keyup.enter="addTask"
      placeholder="Tambahkan kegiatan"
      type="text"
    />
    <button @click="addTask">Tambah</button>

    <p v-if="errorMessage" class="error">{{ errorMessage }}</p>

    <!-- Tombol toggle filter -->
    <div class="filter">
      <label>
        <input type="checkbox" v-model="showOnlyIncomplete" />
        Tampilkan hanya kegiatan yang belum selesai
      </label>
    </div>

    <ul v-if="filteredTasks.length > 0">
      <li v-for="(task, index) in filteredTasks" :key="index" class="task-item">
        <div class="task-content">
          <input type="checkbox" v-model="task.completed" @change="toggleTask(index)" />
          <span :class="{ completed: task.completed }">{{ task.text }}</span>
        </div>
        <button class="delete-button" @click="deleteTask(index)">Hapus</button>
      </li>
    </ul>
    <p v-else>Tidak ada kegiatan yang ditampilkan.</p>
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
input[type="text"] {
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
li.task-item {
  text-align: left;
  margin-bottom: 10px;
  padding: 8px;
  background: #f2f2f2;
  border-radius: 5px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.task-content {
  display: flex;
  align-items: center;
  gap: 10px;
}
.completed {
  text-decoration: line-through;
  color: gray;
}
.delete-button {
  background-color: #ff4d4d;
  border: none;
  color: white;
  padding: 6px 10px;
  border-radius: 5px;
  cursor: pointer;
}
.delete-button:hover {
  background-color: #e60000;
}
.error {
  color: red;
  margin-top: 10px;
}
.filter {
  margin-top: 15px;
}
</style>
