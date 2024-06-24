<template>
  <div id="app">
    <header>
      <h1>Weather App</h1>
    </header>
    <main>
      <input
        v-model="location"
        type="text"
        placeholder="Enter city or postal code"
      />
      <button @click="fetchWeather">Search</button>
      <div v-if="error" class="error">{{ error }}</div>
      <div v-if="weather" class="weather-info">
        <p>Temperature: {{ weather.main.temp }} °C</p>
        <p>Humidity: {{ weather.main.humidity }} %</p>
        <p>Wind Speed: {{ weather.wind.speed }} m/s</p>
        <p>Description: {{ weather.weather[0].description }}</p>
      </div>
    </main>
  </div>
</template>

<script>
import axios from 'axios';

const API_KEY = '88d45c7274a911c3111004188c16f6cd';

export default {
  name: 'App',
  data() {
    return {
      location: '',
      weather: null,
      error: null
    };
  },
  methods: {
    async fetchWeather() {
      try {
        const response = await axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.location}&appid=${API_KEY}&units=metric`);
        this.weather = response.data;
        this.error = null;
      } catch (err) {
        this.error = 'Could not fetch weather data. Please check the location and try again.';
        this.weather = null;
      }
    }
  }
};
</script>

<style scoped>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  margin-top: 60px;
}

header {
  font-size: 2em;
  margin-bottom: 20px;
}

input {
  padding: 10px;
  margin: 10px;
  border-radius: 5px;
  border: 1px solid #ccc;
}

button {
  padding: 10px 20px;
  margin: 10px;
  border-radius: 5px;
  background-color: #42b983;
  color: white;
  border: none;
  cursor: pointer;
}

button:hover {
  background-color: #369f77;
}

.weather-info {
  margin-top: 20px;
}

.error {
  color: red;
  margin-top: 20px;
}
</style>
