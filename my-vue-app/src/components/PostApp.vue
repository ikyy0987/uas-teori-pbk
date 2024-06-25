<template>
  <div class="post-app">
    <h1>Postingan Pengguna</h1>
    <div class="filters">
      <label for="userSelect">Pilih Pengguna:</label>
      <select v-model="selectedUser" @change="loadPosts">
        <option v-for="user in users" :value="user.id">{{ user.name }}</option>
      </select>
    </div>
    <div v-if="posts.length > 0">
      <ul>
        <li v-for="post in posts" :key="post.id">
          <h3>{{ post.title }}</h3>
          <p>{{ post.body }}</p>
        </li>
      </ul>
    </div>
    <div v-else>
      <p>Tidak ada postingan untuk pengguna yang dipilih.</p>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';

const selectedUser = ref('');
const users = ref([]);
const posts = ref([]);

function loadUsers() {
  fetch('https://jsonplaceholder.typicode.com/users')
    .then(response => response.json())
    .then(data => {
      users.value = data;
      if (data.length > 0) {
        selectedUser.value = data[0].id; // Memilih pengguna pertama sebagai default
        loadPosts();
      }
    })
    .catch(error => console.error('Error fetching users:', error));
}

function loadPosts() {
  if (!selectedUser.value) return;
  fetch(`https://jsonplaceholder.typicode.com/posts?userId=${selectedUser.value}`)
    .then(response => response.json())
    .then(data => {
      posts.value = data;
    })
    .catch(error => console.error('Error fetching posts:', error));
}

onMounted(() => {
  loadUsers();
});
</script>

<style scoped>
.post-app {
  background-color: #f9f9f9;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

.post-app h1 {
  margin-bottom: 20px;
}

.filters {
  margin-bottom: 20px;
}

.filters label {
  font-weight: bold;
}

.filters select {
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.filters select:focus {
  outline: none;
}

.posts-list {
  list-style: none;
  padding: 0;
}

.posts-list li {
  margin-bottom: 20px;
  border-bottom: 1px solid #ccc;
}

.posts-list li:last-child {
  border-bottom: none;
}

.posts-list h3 {
  margin-bottom: 10px;
}

.posts-list p {
  margin: 0;
}
</style>
