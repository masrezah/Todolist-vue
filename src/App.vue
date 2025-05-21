<template>
  <div class="todo-app">
    <h1>To Do List</h1>

    <div class="input-section">
      <input
        v-model="newTaskText"
        @keyup.enter="addTask"
        placeholder="New Task"
      />
      <select v-model="newTaskPriority">
        <option disabled value="">Prioritas</option>
        <option value="tinggi">🔥 Crucial</option>
        <option value="sedang">💼 Work</option>
        <option value="rendah">🍃 No worries</option>
      </select>
      <button @click="addTask">+</button>
    </div>

    <ul>
      <li
        v-for="(task, index) in sortedTasks"
        :key="index"
        :class="['task-item', task.priority, { completed: task.completed }]"
        @click="toggleTask(index)"
      >
        <span class="task-text">{{ task.text }}</span>
        <span class="priority" :class="task.priority">
          <template v-if="task.priority === 'tinggi'">🔥</template>
          <template v-else-if="task.priority === 'sedang'">💼</template>
          <template v-else>🍃</template>
        </span>
      </li>
    </ul>

    <div class="actions">
      <button @click="clearCompleted">Clear completed</button>
      <button @click="clearAll">Clear all</button>
    </div>

    <p>Pending Tasks: {{ pendingTasks }}</p>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const newTaskText = ref('')
const newTaskPriority = ref('')
const tasks = ref([])

const addTask = () => {
  const text = newTaskText.value.trim()
  const priority = newTaskPriority.value
  if (text !== '' && priority !== '') {
    tasks.value.push({ text, completed: false, priority })
    newTaskText.value = ''
    newTaskPriority.value = ''
  }
}

const toggleTask = (index) => {
  tasks.value[index].completed = !tasks.value[index].completed
}

const removeTask = (index) => {
  tasks.value.splice(index, 1)
}

const clearCompleted = () => {
  tasks.value = tasks.value.filter(task => !task.completed)
}

const clearAll = () => {
  tasks.value = []
}

const pendingTasks = computed(() =>
  tasks.value.filter(task => !task.completed).length
)

const priorityOrder = { tinggi: 1, sedang: 2, rendah: 3 }

const sortedTasks = computed(() => {
  return tasks.value.slice().sort((a, b) => {
    return priorityOrder[a.priority] - priorityOrder[b.priority]
  })
})
</script>
