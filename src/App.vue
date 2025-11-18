<template>
  <div class="app">
    <h1>Weather App🌤️</h1>

    <div class="search">
      <input 
        type="text" 
        v-model="city" 
        placeholder="Digite o nome da cidade..."
        @keyup.enter="getWeather"
      />
      <button @click="getWeather">Buscar</button>
    </div>

    <div v-if="weather" class="weather-box">
      <h2>{{ weather.name }}, {{ weather.sys.country }}</h2>
      <p class="temp">{{ Math.round(weather.main.temp) }}°C</p>
      <p class="desc">{{ weather.weather[0].description }}</p>
    </div>

    <p v-if="error" class="error">{{ error }}</p>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import axios from 'axios'

const city = ref('')
const weather = ref(null)
const error = ref('')

const API_KEY = 'dc62924456f5f88778f3068df0555e1a'
const BASE_URL = 'https://api.openweathermap.org/data/2.5/weather'

async function getWeather() {
  if (!city.value) return
  try {
    error.value = ''
    const res = await axios.get(BASE_URL, {
      params: {
        q: city.value,
        appid: API_KEY,
        units: 'metric',
        lang: 'pt_br'
      }
    })
    weather.value = res.data
  } catch (err) {
    error.value = 'Cidade não encontrada 😢'
    weather.value = null
  }
}
</script>

<style scoped>
.app {
  text-align: center;
  font-family: 'Arial', sans-serif;
  padding: 2rem;
  max-width: 400px;
  margin: 0 auto;
}
.search {
  display: flex;
  justify-content: center;
  gap: 10px;
  margin-bottom: 1rem;
}
input {
  padding: 0.5rem;
  width: 70%;
  border: 1px solid #ccc;
  border-radius: 6px;
  background-color: #ccc;
  color: #000;
}
button {
  padding: 0.5rem 1rem;
  background: #66baff;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
}
button:hover {
  background: #4ca6e0;
}
.weather-box {
  background: #e0f7ff;
  padding: 1rem;
  border-radius: 10px;
  margin-top: 1rem;
  box-shadow: 0 2px 5px rgba(0,0,0,0.1);
  color: #000;
}
.temp {
  font-size: 2rem;
  font-weight: bold;
}
.error {
  color: red;
  margin-top: 1rem;
}
</style>
