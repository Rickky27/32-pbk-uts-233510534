<template>
  <div class="box-main">
    <div class="card-box">
      <h1 class="title">📋 Daftar Kegiatan</h1>

      <div class="layout">
        <!-- Input Kegiatan -->
        <div class="Input-kegiatan">
          <input
            v-model="newTask"
            @keyup.enter="addTask"
            type="text"
            placeholder="Tambah kegiatan baru..."
            class="input-box"
          />
          <button @click="addTask" class="click-box">Tambah</button>
        </div>

        <!-- Filter -->
        <div class="Filter">
          <label>
            <input type="checkbox" v-model="showOnlyUnfinished" class="mr-2" />
            Tampilkan hanya yang belum selesai
          </label>
        </div>

        <!-- Daftar Kegiatan -->
        <ul class="task-list">
          <li
            v-for="(task, index) in filteredTasks"
            :key="index"
            class="main-List"
          >
            <div class="List">
              <input type="checkbox" v-model="task.done" class="checkbox" />
              <span :class="{ 'task-done': task.done, 'task-pending': !task.done }">
                {{ task.name }}
              </span>
            </div>
            <button @click="removeTask(index)" class="btn-cancel">Batalkan</button>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'

const newTask = ref('')
const tasks = ref([])
const showOnlyUnfinished = ref(false)

// Data dummy saat pertama kali load
onMounted(() => {
  tasks.value = [
    { name: 'makan telur rebus', done: false },
    { name: 'Minum jus', done: false },
    { name: 'Olahraga', done: false },
    { name: 'Belajar', done: true }
  ]
})

const addTask = () => {
  if (newTask.value.trim()) {
    tasks.value.push({ name: newTask.value.trim(), done: false })
    newTask.value = ''
  }
}

const removeTask = (index) => {
  tasks.value.splice(index, 1)
}

const filteredTasks = computed(() => {
  return showOnlyUnfinished.value
    ? tasks.value.filter(task => !task.done)
    : tasks.value
})
</script>

<style scoped>
/* Layout utama */
.box-main {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background:  #ffffff ;
  padding: 2rem;
}

.card-box {
  width: 100%;
  max-width: 600px;
  background-color: #ffffff;
  border-radius: 1rem;
  padding: 2rem;
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.1);
}

/* Judul */
.title {
  font-size: 2.5rem;
  text-align: center;
  margin-bottom: 2rem;
  color: #4f46e5;
}

/* Input */
.Input-kegiatan {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 0.5rem;
  margin-bottom: 1.5rem;
}

.input-box {
  padding: 0.5rem 1rem;
  border-radius: 0.5rem;
  border: 1px solid #ccc;
  flex: 1;
  max-width: 300px;
  font-size: 1rem;
}

.click-box {
  background-color: #4f46e5;
  color: white;
  border: none;
  border-radius: 0.5rem;
  padding: 0.5rem 1.2rem;
  font-weight: 500;
  cursor: pointer;
  transition: background-color 0.3s;
}
.click-box:hover {
  background-color: #4338ca;
}

/* Filter */
.Filter {
  text-align: center;
  font-size: 0.9rem;
  color: #333;
  margin-bottom: 1.5rem;
}

/* Daftar Kegiatan */
.task-list {
  list-style: none;
  padding: 0;
}

.main-List {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #f3f4f6;
  border-radius: 0.5rem;
  padding: 0.75rem 1rem;
  margin-bottom: 0.75rem;
  transition: transform 0.2s ease;
}
.main-List:hover {
  transform: scale(1.01);
  background-color: #e5e7eb;
}

.List {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.checkbox {
  width: 20px;
  height: 20px;
}

.task-done {
  text-decoration: line-through;
  color: #9ca3af;
  font-style: italic;
}

.task-pending {
  color: #111827;
  font-weight: 500;
}

.btn-cancel {
  background: none;
  color: #ef4444;
  border: none;
  font-size: 0.85rem;
  cursor: pointer;
}
.btn-cancel:hover {
  text-decoration: underline;
}
</style>
