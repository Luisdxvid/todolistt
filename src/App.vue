<script setup>
import { ref, onMounted, watch } from 'vue'

const newTodo = ref('')
const todos = ref([])

// Load todos from localStorage
onMounted(() => {
  const savedTodos = localStorage.getItem('todos')
  if (savedTodos) {
    todos.value = JSON.parse(savedTodos)
  }
})

// Save todos to localStorage whenever it changes
watch(todos, (newTodos) => {
  localStorage.setItem('todos', JSON.stringify(newTodos))
}, { deep: true })

function addTodo() {
  if (newTodo.value.trim()) {
    todos.value.push({
      id: Date.now(),
      text: newTodo.value.trim(),
      completed: false
    })
    newTodo.value = ''
  }
}

function toggleTodo(todo) {
  todo.completed = !todo.completed
}

function removeTodo(id) {
  todos.value = todos.value.filter(todo => todo.id !== id)
}
</script>

<template>
  <div class="todo-app">
    <h1>Vue.js Todo List</h1>
    <div class="add-todo">
      <input 
        v-model="newTodo" 
        @keyup.enter="addTodo"
        placeholder="Add a new todo"
      />
      <button @click="addTodo">Add</button>
    </div>
    <ul class="todo-list">
      <li v-for="todo in todos" :key="todo.id" :class="{ completed: todo.completed }">
        <span @click="toggleTodo(todo)">{{ todo.text }}</span>
        <button @click="removeTodo(todo.id)" class="remove-btn">X</button>
      </li>
    </ul>
  </div>
</template>

<style scoped>
.todo-app {
  max-width: 400px;
  margin: 0 auto;
  padding: 20px;
  font-family: Arial, sans-serif;
  background-color: #1e1e1e;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
  color: #e0e0e0;
}

h1 {
  color: #42b883;
  text-align: center;
  margin-bottom: 20px;
}

.add-todo {
  display: flex;
  margin-bottom: 20px;
}

input {
  flex-grow: 1;
  padding: 10px;
  font-size: 16px;
  border: 1px solid #444;
  border-radius: 4px 0 0 4px;
  background-color: #2a2a2a;
  color: #e0e0e0;
}

input::placeholder {
  color: #888;
}

button {
  padding: 10px 15px;
  background-color: #42b883;
  color: #1e1e1e;
  border: none;
  cursor: pointer;
  font-size: 16px;
  border-radius: 0 4px 4px 0;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #3aa876;
}

.todo-list {
  list-style-type: none;
  padding: 0;
}

li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  background-color: #2a2a2a;
  border: 1px solid #444;
  margin-bottom: 5px;
  cursor: pointer;
  border-radius: 4px;
  transition: background-color 0.3s;
}

li:hover {
  background-color: #333;
}

li.completed span {
  text-decoration: line-through;
  color: #888;
}

.remove-btn {
  background-color: #ff4d4d;
  color: #1e1e1e;
  border: none;
  padding: 5px 10px;
  cursor: pointer;
  border-radius: 4px;
  font-size: 14px;
  transition: background-color 0.3s;
}

.remove-btn:hover {
  background-color: #e04444;
}
</style>