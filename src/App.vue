<template>
  <div id="app">
    <h1>Kegiatan Pindahan Kos</h1>

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
          <small v-if="task.due" class="task-due">
            ({{ task.due }})
          </small>

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
  { text: 'Bayar uang sewa', done: false, editing: false, due: '2025-05-09' },
  { text: 'Bersihkan kamar lama', done: false, editing: false, due: '2025-05-04'},
  { text: 'Sewa transport barang', done: false, editing: false, due: '2025-05-06' },
  { text: 'Beli perabotan baru', done: false, editing: false, due: '2025-05-07' },
  { text: 'Beli Stock Snack', done: false, editing: false, due: '2025-05-08' },
  { text: 'Bersihkan kamar baru', done: false, editing: false, due: '2025-05-10' },
  { text: 'Packing semua barang', done: false, editing: false, due: '2025-05-11' }
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
