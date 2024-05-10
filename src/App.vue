<template>
    <div class="top-div">
      <h1>My Very Simple Looking To-Do List</h1>
    </div>
  <div class="todo-app">
    <form @submit.prevent="addOrUpdateTodo" class="todo-form">
      <input v-model="newTodo" required placeholder="Masukan TODO" class="todo-input">
      <button type="submit" class="todo-button">{{ editingTodo ? 'Update' : 'Add Todo' }}</button>
    </form>

    <ul class="todo-list">
      <li v-for="todo in filteredTodos" :key="todo.id" class="todo-item">
        <button @click="toggleDone(todo)" class="done-button" :class="{ done: todo.done }">
          {{ todo.done ? 'Undone' : 'Done' }}
        </button>
        <span :class="{ done: todo.done }">{{ todo.text }}</span>
        <div class="button-group">
          <button @click="editTodo(todo)" class="edit-button">Edit</button>
          <button @click="removeTodo(todo)" class="delete-button">Delete</button>
        </div>
      </li>
    </ul>

    <button @click="hideCompleted = !hideCompleted" class="toggle-completed">
      {{ hideCompleted ? 'Show all' : 'Hide Completed' }}
    </button>

    <p>{{ pesan }}</p>
    <div class="copyright">
      &copy; Yardan Hadziq - 223510469
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onUpdated, watch } from 'vue'

const pesan = ref("")

onUpdated(() => {
  console.log("Ada suatu perubaha terjadi di website")
})

let id = 0
const newTodo = ref('')
const hideCompleted = ref(false)
const editingTodo = ref(null)

watch(newTodo, (data) => {
  if (data.includes("?")) {
    pesan.value = "tidak boleh"
  } else {
    pesan.value = ""
  }
})

const todos = ref([
{ id: id++, text: 'Yardan Hadziq', done: false },
  { id: id++, text: '223510469', done: true }
])

const filteredTodos = computed(() => {
  return hideCompleted.value ? todos.value.filter((t) => !t.done) : todos.value
})

function addOrUpdateTodo() {
  if (editingTodo.value) {
    editingTodo.value.text = newTodo.value
    editingTodo.value = null
  } else {
    todos.value.push({ id: id++, text: newTodo.value, done: false })
  }
  newTodo.value = ''
}

function removeTodo(todo) {
  const index = todos.value.indexOf(todo)
  if (index !== -1) {
    todos.value.splice(index, 1)
  }
}

function editTodo(todo) {
  editingTodo.value = todo
  newTodo.value = todo.text
}

function toggleDone(todo) {
  todo.done = !todo.done
}
</script>

<style>
  .top-div {
    text-align: center;
    margin-bottom: 20px;
    background: rgba(0, 0, 0, 0.35);
    border-radius: 20px;
    padding: 30px;
  }

  .top-div h1 {
    color: white;
    font-size: 24px;
  }

  body {
    background: rgb(5,0,40);
    background: radial-gradient(circle, rgba(5,0,40,1) 0%, rgba(34,69,91,1) 80%); 
  }

  .todo-app {
    background: rgba(0, 0, 0, 0.35);
    border-radius: 20px;
    padding: 45px;
  }

  .button-group {
    display: flex;
  }

  .todo-item {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 10px;
  }

  .todo-input {
    margin: 0 50px 5px 1px;
    padding: 15px;
    border-radius: 15px;
  }
  
  .todo-button {
    color: black;
    background-color: cyan;
    padding: 13px;
    border-radius: 15px;
    cursor: pointer;
  }

  .delete-button,
  .edit-button {
    margin-left: 5px;
    padding: 4px 8px;
    cursor: pointer;
    color: white;
    background-color: #dc3545; 
    border: none;
    border-radius: 10px;
  }

  .edit-button {
    background-color: #007bff; 
  }

  .done {
    color: green;
  }

  .done-button {
    margin-right: 1px;
    padding: 4px 8px;
    cursor: pointer;
    color: white;
    background-color: #28a745;
    border: none;
    border-radius: 10px;
    position: relative;
  }

  .done-button.done {
    background-color: #6c757d;
  }

  .copyright {
    position: absolute;
    bottom: 10px;
    right: 10px;
    color: white;
    font-size: 12px;
  }

  .toggle-completed{
    margin-top: 20px;
  }
</style>