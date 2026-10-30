<template>
  <div class="page">
    <h1>Random Dog Image</h1>
    <p v-if="loading">Loading...</p>
    <p v-else-if="error" style="color: red;">{{ error }}</p>

    <img
      v-else-if="imageUrl"
      :src="imageUrl"
      alt="Random dog"
      style="max-width: 100%; height: auto;"
    />

    <br/><br/>
    <button @click="fetchData" :disabled="loading">
      {{ loading ? 'Loading...' : 'Get New Image' }}
    </button>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
const imageUrl = ref(null)
const loading = ref(false)
const error = ref(null)

async function fetchData() {
  loading.value = true
  error.value = null

  try {
    const response = await fetch('https://dog.ceo/api/breeds/image/random')
    if (!response.ok) {
      throw new Error(`There was an error: ${response.status}`)
    }

    const result = await response.json()
    imageUrl.value = result.message
  } catch (err) {
    error.value = err.message || 'Something went wrong'
    imageUrl.value = null
  } finally {
    loading.value = false
  }
}
onMounted(()=>{
  fetchData()
})
</script>

<style>
body {
  margin: 0;
  padding: 0;
}

.page {
  background-color: #6a6b6b;
  min-height: 100vh;
  padding: 20px;
}
</style>