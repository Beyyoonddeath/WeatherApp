<template>
  <div class="wrapper">
    <h1>Weather App</h1>
    <p>Find out the weather in {{ city == '' ? 'your city' : cityName }}</p>
    <input type="text" v-model="city" placeholder="Voronezh">
    <button v-show="city != ''" @click="getWeather()">Get the weather</button>
    <p v-show="error != ''" class="error">{{ error }}</p>
    <div class="info" v-if="info != null">
      <p><b>Temperature, &deg;C:</b> {{ showTemp }}</p>
      <p><b>Feels like, &deg;C:</b> {{ showFeelsLike }}</p>
      <p><b>Minimal temperature, &deg;C:</b> {{ showMinTemp }}</p>
      <p><b>Maximal temperature, &deg;C:</b> {{ showMaxTemp }}</p>
    </div>
  </div>
</template>
<script>
  import axios from 'axios';

  export default {
    data() {
      return {
        city: "",
        error: "",
        info: null
      }
    },

    computed: {
      cityName() {
        return `"${this.city}"`
      },
      
      showTemp() {
        return this.info.main.temp
      },

      showFeelsLike() {
        return this.info.main.feels_like
      },

      showMinTemp() {
        return this.info.main.temp_min
      },

      showMaxTemp() {
        return this.info.main.temp_max
      },
    },

    methods: {
      getWeather() {
        if (this.city.trim().length < 2) {
          this.error = "Invalid city name!";
          return false;
        }
        
        this.error = '';
        axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=08516016502decb046526536a4e913ec`)
          .then(res => (this.info = res.data))
      }
    }
  }
</script>
<style scoped>
  .error {
    color: red;
    background: rgb(200, 10, 10, 0.6);
  }

  .wrapper {
    width: 900px;
    height: 500px;
    border-radius: 50px;
    background: rgb(26, 26, 26);
    padding: 20px;
    text-align: center;
    color: white;
  }

  .wrapper h1 {
    margin-top: 50px;
  }

  .wrapper p {
    margin-top: 10px;
  }

  .wrapper input {
    margin-top: 30px;
    background: rgb(40, 40, 40);
    border: 0;
    border-bottom: 2px solid #110813;
    color: #fcfcfc;
    font-size: 14px;
    padding: 5px 8px;
    outline: none;
    border-radius: 10px;
    transition: all 1s ease;
    text-align: center;
  }

  .wrapper button {
    margin-left: 15px;
    background-color: rgb(40, 40, 40);
    border: 0;
    border-bottom: 2px solid #110813;
    color: #676767;
    font-size: 14px;
    padding: 5px 8px;
    outline: none;
    border-radius: 7px;
    transition: all 1s ease;
    text-align: center;
    cursor: pointer;
  }

  .wrapper input:focus, .wrapper button:hover {
    background: rgb(59, 59, 59);
    border-bottom-color: #24b4b6;
    color: #fcfcfc;
    transform: scale(1.01) translateY(-2px);
  }

  .wrapper .info {
    margin-top: 30px;
    background-color: rgba(59, 59, 59, 0.149);
    border-radius: 10px;
    padding-top: 10px;
    padding-bottom: 20px;
  }
</style>