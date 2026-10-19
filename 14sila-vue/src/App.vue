<script setup>
import { onMounted, ref } from 'vue'
import api from './services/api'

const users = ref([])
const isLoading = ref(true)
const errorMessage = ref('')

async function loadUsers() {
  isLoading.value = true
  errorMessage.value = ''

  try {
    const response = await api.get('/api/users')
    users.value = response.data.data
  } catch (error) {
    errorMessage.value =
      error instanceof Error ? error.message : 'Terjadi kesalahan saat mengambil data.'
  } finally {
    isLoading.value = false
  }

  console.log(response.data)
}

onMounted(loadUsers)
</script>

<template>
  <main class="container">
    <h1 class="page-title">Daftar Pengguna</h1>

    <p v-if="isLoading">Memuat data...</p>
    <p v-else-if="errorMessage" class="error">
      Gagal mengambil data: {{ errorMessage }}
    </p>
    <p v-else-if="users.length === 0">Data pengguna tidak ditemukan.</p>

    <ul v-else class="user-list">
      <li v-for="user in users" :key="user.id" class="user-card">
        <h2>{{ user.name }}</h2>
        <p>{{ user.email }}</p>
      </li>
    </ul>
  </main>
</template>

<style>

* {
  box-sizing: border-box;
}

body {
  margin: 0;
  background: #f3f4f6;
  color: #1f2937;
  font-family: Arial, sans-serif;
  line-height: 1.6;
}

.container {
  width: min(800px, calc(100% - 32px));
  margin: 40px auto;
}

.page-title {
  margin: 0 0 28px;
  text-align: center;
  color: #111827;
  font-size: 30px;
  font-weight: 700;
  letter-spacing: -0.5px;
}

.user-list {
  display: grid;
  gap: 14px;
  padding: 0;
  list-style: none;
}

.user-card {
  padding: 22px 24px;
  background: #ffffff;
  border: 1px solid #e5e7eb;
  border-left: 4px solid #4f46e5;
  border-radius: 10px;
  box-shadow: 0 3px 10px rgb(0 0 0 / 5%);
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.user-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 16px rgb(0 0 0 / 8%);
}

.user-card h2 {
  margin: 0 0 8px;
  color: #111827;
  font-size: 19px;
  font-weight: 700;
  line-height: 1.4;
  text-transform: capitalize;
  overflow-wrap: anywhere;
}

.user-card p {
  margin: 0;
  color: #4b5563;
  font-size: 15px;
  font-weight: 500;
  overflow-wrap: anywhere;
}

.error {
  padding: 14px 16px;
  color: #b91c1c;
  background: #fee2e2;
  border-radius: 8px;
  font-size: 14px;
  font-weight: 500;
}

p[v-if],
p[v-else-if] {
  font-size: 15px;
}

@media (max-width: 480px) {
  .container {
    margin: 28px auto;
  }

  .page-title {
    font-size: 25px;
    margin-bottom: 22px;
  }

  .user-card {
    padding: 18px;
  }

  .user-card h2 {
    font-size: 17px;
  }

  .user-card p {
    font-size: 14px;
  }
}

</style>