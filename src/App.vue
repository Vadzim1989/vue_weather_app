<template>
  <div className="wrapper">
    <h1>Weather APP</h1>
    <p>Weather in the {{ city == '' ? 'city' : cityName }}</p>
    <input type="text" placeholder="Enter your city..." v-model="city">
    <button v-if="city" @click="getWeather()">Get weather</button>
    <button disabled v-else>Get weather</button>
    <p v-show="error" className="error">{{ error }}</p>
    <div v-if="weather != null" className="weather">
      <p>{{ showTemp }}</p>
      <p>{{ showFeelsLike }}</p>
      <p>{{ showMinTemp }}</p>
      <p>{{ showMaxTemp }}</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  data() {
    return {
      city: '',
      error: '',
      weather: null
    }
  },
  computed: {
    cityName() {
      return `"${this.city}"`;
    },
    showTemp() {
      return `Temperature: ${this.weather.temp}`;
    },
    showFeelsLike() {
      return `Feels like: ${this.weather.feels_like}`;
    },
    showMinTemp() {
      return `Min temperature: ${this.weather.temp_min}`;
    },
    showMaxTemp() {
      return `Max temperature: ${this.weather.temp_max}`;
    }
  },
  methods: {
    getWeather() {
      if(this.city.trim().length < 2) {
        this.error = 'Enter correct city name';
        return false;
      }
      this.error = '';
      axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=ad18e8fc8e12387d062b778a73224c81`)
      .then(res => this.weather = res.data.main)
    }
  }
}
</script>

<style scoped>

.weather {
  width: 20rem;
  margin: 4rem auto;
  border-radius: 10px;
  background: rgb(0,0,0);
  background: linear-gradient(90deg, #1f0f24 50%, rgba(102,107,12,1) 100%);
}

.error {
  color: #d03939;
}

.wrapper {
  width: 900px;
  height: 500px;
  border-radius: 50px;
  background: #1f0f24;
  padding: 20px;
  text-align: center;
  color: white;
}

.wrapper h1 {
  margin-top: 50px;
}

.wrapper p {
  margin-top: 20px;
}

.wrapper input {
  margin-top: 30px;
  background: transparent;
  border: 0;
  border-bottom: 2px solid #110813;
  color: #fcfcfc;
  font-size: 14px;
  padding: 5px 8px;
  outline: none;
}

.wrapper input:focus {
  border-bottom-color: #6e2d7d;
}

.wrapper button {
  background: #e3bc4b;
  color: #fff;
  border-radius: 10px;
  border: 2px solid #b99935;
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
  transition: transform 500ms ease;
}

.wrapper button:hover {
  transform: scale(1.1) translateY(-5px);
}

.wrapper button:disabled {
  background: #e6d5a5;
  cursor: not-allowed;
}
</style>