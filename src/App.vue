<script setup>
import { ref, onMounted } from 'vue'

const data = ref(null)
const loading = ref(false)
const error = ref(null)

const fetchData = async () => {
  loading.value = true
  error.value = null
  try {
    const response = await fetch('/api/test')
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`)
    }
    data.value = await response.json()
  } catch (e) {
    error.value = e.message
  } finally {
    loading.value = false
  }
}

onMounted(() => {
  fetchData()
})
</script>

<template>
  <div class="container">
    <h1>Vue + Vite Fetch App</h1>
    
    <div class="card">
      <h2>Fetch da /api/test</h2>
      
      <button @click="fetchData" :disabled="loading">
        {{ loading ? 'Caricamento...' : 'Ricarica' }}
      </button>
      
      <div v-if="loading" class="loading">
        Caricamento dati...
      </div>
      
      <div v-else-if="error" class="error">
        Errore: {{ error }}
      </div>
      
      <div v-else-if="data" class="success">
        <h3>Risposta ricevuta:</h3>
        <pre>{{ JSON.stringify(data, null, 2) }}</pre>
      </div>
    </div>
  </div>
</template>

<style scoped>
.container {
  max-width: 800px;
  margin: 0 auto;
  padding: 2rem;
}

h1 {
  color: #42b883;
  text-align: center;
}

.card {
  background: #f9f9f9;
  border-radius: 8px;
  padding: 2rem;
  margin-top: 2rem;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

button {
  background: #42b883;
  color: white;
  border: none;
  padding: 0.75rem 1.5rem;
  border-radius: 4px;
  cursor: pointer;
  font-size: 1rem;
  margin-bottom: 1rem;
}

button:hover:not(:disabled) {
  background: #35a372;
}

button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.loading {
  color: #666;
  padding: 1rem;
}

.error {
  color: #e74c3c;
  padding: 1rem;
  background: #ffebee;
  border-radius: 4px;
}

.success {
  padding: 1rem;
}

pre {
  background: #2c3e50;
  color: #42b883;
  padding: 1rem;
  border-radius: 4px;
  overflow-x: auto;
}
</style>
