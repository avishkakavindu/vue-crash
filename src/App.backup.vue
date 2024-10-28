<script setup>
import { ref, onMounted } from 'vue'

const name = ref('John Doe')
const status = ref('active')
const tasks = ref(['Task 1', 'Task 2', 'Task 3'])
const newTask = ref('')

const toggleStatus = () => {
  status.value = status.value === 'active' ? 'pending' : 'active'
}

const addTask = () => {
  if (newTask.value.trim() !== '') {
    tasks.value.push(newTask.value)
    newTask.value = ''
  }
}

const deleteTask = idx => {
  tasks.value.splice(idx, 1)
}

onMounted(async () => {
  try {
    const res = await fetch('https://jsonplaceholder.typicode.com/todos')
    const data = await res.json()
    tasks.value = data.map(task => task.title)
  } catch (error) {
    console.log('Error fetching tasks:', error)
  }
})
</script>

<template>
  <h1>{{ name }}</h1>
  <p v-if="status === 'active'">User is Active</p>
  <p v-else-if="status === 'pending'">User is Pending</p>
  <p v-else>User is Inactive</p>

  <form @submit.prevent="addTask">
    <label for="newTask">New Task:</label>
    <input type="text" id="newTask" name="newTask" v-model="newTask" />
    <button type="submit">Add Task</button>
  </form>

  <h3>Tasks</h3>
  <ul>
    <li v-for="(task, idx) in tasks" :key="task">
      <span>{{ task }}</span>
      <button @click="deleteTask(idx)">Delete</button>
    </li>
  </ul>

  <button @click="toggleStatus">Change status</button>
</template>
