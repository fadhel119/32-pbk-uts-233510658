<template>
  <div id="app">
    <h1>Checklist Pindahan Kos</h1>

    <div class="input-container">
      <input v-model="newTask" @keyup.enter="addTask" type="text" placeholder="Tambahkan kegiatan..." />
      <button @click="addTask">Tambah</button>
      <button class="reset" @click="resetTasks">Reset Semua</button>
    </div>

    <ul>
      <li v-for="(task, index) in filteredTasks" :key="index">
        <input type="checkbox" v-model="task.done" />

        <template v-if="task.editing">
          <input v-model="task.text" @keyup.enter="saveEdit(task)" class="edit-input" />
          <button @click="saveEdit(task)">Simpan</button>
        </template>
        <template v-else>
          <span :style="{ textDecoration: task.done ? 'line-through' : 'none', color: task.done ? 'gray' : 'black' }">
            {{ task.text }}
          </span>
          <button @click="editTask(task)">Edit</button>
        </template>

        <button @click="confirmDelete(index)">Batalkan</button>
      </li>
    </ul>

    <button class="toggle-filter" @click="showOnlyIncomplete = !showOnlyIncomplete">
      {{ showOnlyIncomplete ? 'Tampilkan Semua Kegiatan' : 'Tampilkan Belum Selesai' }}
    </button>

    <footer class="footer">
      <p>Made by Fadellll</p>
    </footer>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const newTask = ref('');
const showOnlyIncomplete = ref(false);

const tasks = ref([
  { text: 'Bayar uang sewa', done: false, editing: false },
  { text: 'Bersihkan kamar lama', done: false, editing: false },
  { text: 'Sewa transport barang', done: false, editing: false },
  { text: 'Packing semua barang', done: false, editing: false },
  { text: 'Ambil kunci kamar baru', done: false, editing: false }
]);

function addTask() {
  if (newTask.value.trim() === '') {
    alert('Kegiatan tidak boleh kosong!');
    return;
  }
  tasks.value.push({ text: newTask.value, done: false, editing: false });
  newTask.value = '';
}

function removeTask(index) {
  tasks.value.splice(index, 1);
}

function confirmDelete(index) {
  if (confirm('Yakin ingin menghapus kegiatan ini?')) {
    removeTask(index);
  }
}

function resetTasks() {
  if (confirm('Yakin ingin menghapus semua kegiatan?')) {
    tasks.value = [];
  }
}

function editTask(task) {
  task.editing = true;
}

function saveEdit(task) {
  if (task.text.trim() === '') {
    alert('Teks kegiatan tidak boleh kosong!');
    return;
  }
  task.editing = false;
}

const filteredTasks = computed(() =>
  showOnlyIncomplete.value ? tasks.value.filter(task => !task.done) : tasks.value
);
</script>

<style>
* {
  box-sizing: border-box;
}
body {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background: #e3f2fd;
  color: #333;
  margin: 0;
  padding: 0;
}
#app {
  max-width: 600px;
  margin: 3rem auto;
  background: #fff;
  padding: 2rem;
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}
h1 {
  text-align: center;
  color: #0277bd;
}
.input-container {
  display: flex;
  gap: 0.5rem;
  margin-bottom: 1.5rem;
}
input[type="text"],
.edit-input {
  flex: 1;
  padding: 0.75rem;
  font-size: 1rem;
  border: 2px solid #ccc;
  border-radius: 8px;
}
.edit-input {
  max-width: 60%;
}
input[type="text"]:focus,
.edit-input:focus {
  border-color: #0277bd;
  outline: none;
}
ul {
  list-style: none;
  padding: 0;
}
li {
  display: flex;
  align-items: center;
  justify-content: space-between;
  background: #f9f9f9;
  margin-bottom: 0.5rem;
  padding: 0.75rem;
  border-radius: 10px;
  gap: 0.5rem;
}
input[type="checkbox"] {
  margin-right: 0.5rem;
  transform: scale(1.2);
}
span {
  flex-grow: 1;
}
button {
  background-color: #0277bd;
  color: white;
  border: none;
  padding: 0.4rem 0.8rem;
  border-radius: 6px;
  cursor: pointer;
  font-size: 0.85rem;
}
button:hover {
  background-color: #01579b;
}
button.toggle-filter {
  display: block;
  margin: 2rem auto 0;
  background-color: #01579b;
}
button.reset {
  background-color: #d32f2f;
}
button.reset:hover {
  background-color: #b71c1c;
}
.footer {
  margin-top: 2rem;
  text-align: center;
  color: #999;
  font-size: 0.85rem;
}
</style>
