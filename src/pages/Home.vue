<script setup>
import { ref, computed, onMounted, watch } from 'vue'
import Header from '@/components/Header.vue'
import TaskTable from '@/components/Table.vue'
import Modal from '@/components/Modal.vue'

const tasks = ref([])
const search = ref('')
const sort = ref('date')
const showModal = ref(false)

onMounted(() => {
  const saved = localStorage.getItem('tasks')
  if (saved) {
    tasks.value = JSON.parse(saved)
  }
})

watch(tasks, (newVal) => {
  localStorage.setItem('tasks', JSON.stringify(newVal))
}, { deep: true })

const filteredTasks = computed(() => {
  let result = [...tasks.value]

  // Поиск по всем полям задачи
  if (search.value) {
    const term = search.value.toLowerCase()
    result = result.filter(task =>
      task.title.toLowerCase().includes(term) ||
      task.createdAt.toLowerCase().includes(term) ||
      (task.status ? 'выполнено' : 'не выполнено').includes(term)
    )
  }

  // Сортировка
  if (sort.value === 'date') {
    result.sort((a, b) => new Date(a.createdAt) - new Date(b.createdAt))
  } else if (sort.value === 'status') {
    result.sort((a, b) => a.status - b.status)
  }

  return result
})

const addTask = (task) => {
  tasks.value.push(task)
}

const toggleStatus = (id) => {
  const task = tasks.value.find(t => t.id === id)
  if (task) task.status = !task.status
}

</script>

<template>
  <div class="home">
    <Header
      :search="search"
      :sort="sort"
      @update:search="val => search = val"
      @update:sort="val => sort = val"
      @open-modal="showModal = true"
    />

    <TaskTable :tasks="filteredTasks" @toggle-status="toggleStatus" />

    <Modal v-if="showModal" @close="showModal = false" @create-task="addTask" />
  </div>
</template>