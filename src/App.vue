<template>
  <div :class="['app', weatherClass]">
    <header>
      <h1>Weather Search</h1>
    </header>
    <main>
      <input
        v-model="location"
        type="text"
        placeholder="Enter city or postal code"
        @keyup.enter="fetchWeather"
      />
      <button @click="fetchWeather">Search</button>
      <div v-if="error" class="error">{{ error }}</div>
      <div v-if="weather" class="weather-info">
      <img :src="weatherIconUrl" :alt="weather.weather[0].description" class="weather-icon" />
        <h2>{{ locationName }}</h2>
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
      locationName: '',
      weather: null,
      error: null
    };
  },
  computed: {
    weatherClass() {
      if (!this.weather) return '';
      const description = this.weather.weather[0].description.toLowerCase();
      if (description.includes('clear')) {
        return 'sunny';
      } else if (description.includes('cloud')) {
        return 'cloudy';
      } else if (description.includes('rain')) {
        return 'rainy';
      } else if (description.includes('Snow')) {
        return 'snow';
      }else if (description.includes('haze')) {
        return 'haze';
      }
      else {
        return '';
      }
    },
    weatherIconUrl() {
      if (!this.weather) return '';
      const icon = this.weather.weather[0].icon;
      return `http://openweathermap.org/img/wn/${icon}@2x.png`;
    }
  },
  methods: {
    async fetchWeather() {
      try {
        const response = await axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.location}&appid=${API_KEY}&units=metric`);
        this.weather = response.data;
        this.locationName = `${response.data.name}, ${response.data.sys.country}`;
        this.error = null;
      } catch (err) {
        this.error = 'Could not fetch weather data. Please check the location and try again.';
        this.weather = null;
        this.locationName = '';
      }
    }
  }
};
</script>

<style scoped>
html, body {
  margin: 0;
  padding: 0;
  height: 100%;
  font-family: Avenir, Helvetica, Arial, sans-serif;
}

.app {
  text-align: center;
  min-height: 100vh;
  transition: background 0.5s;
  background-image: url('@/assets/Earth.png');
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.sunny {
  background-image: url('@/assets/Sunny.png');
}

.cloudy {
  background-image: url('@/assets/Cloudy.png');
}

.rainy {
  background-image: url('@/assets/Rain.png');
}

.haze {
  background-image: url('@/assets/Haze.png');
}

.snow {
  background-image: url('@/assets/Snow.png');
}

header {
  font-size: 2em;
  margin: 20px 0;
  height: 100px;
}

main {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  max-width: 600px;
  padding: 20px;
  
  border-radius: 10px;
  margin: 20px;
}

input {
  padding: 10px;
  margin: 10px;
  border-radius: 5px;
  border: 1px solid #ccc;
  width: 100%;
  max-width: 280px;
  box-sizing: border-box;
}

button {
  padding: 10px 20px;
  margin: 10px;
  border-radius: 5px;
  background-color: #42b983;
  color: white;
  border: none;
  cursor: pointer;
  width: 100%;
  max-width: 300px;
}

button:hover {
  background-color: #369f77;
}

.weather-info {
  margin-top: 20px;
  padding: 20px;
  border-radius: 5px;
  width: 100%;
  box-sizing: border-box;
}

.weather-icon {
  width: 100px;
  height: 100px;
  margin-top: 10px;
}

.error {
  color: red;
  margin-top: 20px;
}

@media (max-width: 600px) {
  header {
    font-size: 1.5em;
  }

  main {
    padding: 10px;
  }

  input, button {
    max-width: 100%;
  }

  .weather-icon {
    width: 80px;
    height: 80px;
  }
}
</style>
