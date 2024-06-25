<template>
  <div class="todo-app">
    <h1>Todo List</h1>
    <input v-model="newTask" @keyup.enter="addTask" placeholder="Tambah tugas baru" />
    <ul>
      <li v-for="task in todos" :key="task.id">
        <input type="checkbox" v-model="task.completed" @change="toggleTask(task)" />
        <span :class="{ completed: task.completed }">{{ task.title }}</span>
        <button @click="editTask(task)">Edit</button>
        <button @click="removeTask(task)">Hapus</button>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const todos = ref([]);
const newTask = ref('');

function addTask() {
  if (newTask.value.trim() === '') return;
  todos.value.push({ id: Date.now(), title: newTask.value, completed: false });
  newTask.value = '';
  saveData();
}

function toggleTask(task) {
  task.completed = !task.completed;
  saveData();
}

function removeTask(task) {
  todos.value = todos.value.filter(t => t.id !== task.id);
  saveData();
}

function editTask(task) {
  const newTitle = prompt('Edit task title', task.title);
  if (newTitle !== null) {
    task.title = newTitle;
    saveData();
  }
}

function saveData() {
  localStorage.setItem('todos', JSON.stringify(todos.value));
}

function loadData() {
  const savedTodos = localStorage.getItem('todos');
  if (savedTodos) {
    todos.value = JSON.parse(savedTodos);
  }
}

loadData();
</script>
