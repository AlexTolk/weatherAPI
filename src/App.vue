<script>
  import axios from 'axios'
  export default {
    data() {
      return {
        city: "",
        error: "",
        info: null
      }
    },
    computed: {
      showTemp() {
        return "Temperature: " + this.info.main.temp
      },
      showFeelsLike() {
        return "Feels like: " + this.info.main.feels_like
      },
      showMinTemp() {
        return "Minimal temperature today: " + this.info.main.temp_min
      },
      showMaxTemp() {
        return "Maximum temperature today: " + this.info.main.temp_max
      }
    },
    methods: {
      getWeather() {
        if(this.city.trim().length < 2){
          this.error = "At least one symbol"
          return false
        }

        this.error = ""
        axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=babcf2f5a48399ca552505e591826f0e`)
          .then((res) => this.info = res.data)
      }
    }
  }
</script>

<template>
  <header>

  </header>

  <main>
    <div class="wrapper">
      <h1>Weather APP</h1>
      <p>What is the weather like in {{ city=="" ? "your city " : city }}?</p>
      <input type="text" v-model="city" placeholder="City">
      <button v-if="city !== ''" @click="getWeather()">See the weather</button>
      <button disabled v-else>Which city?</button>
      <p class="error">{{ error }}</p>

      <div v-if="info !== null">
        <p>{{ showTemp }}</p>
        <p>{{ showFeelsLike }}</p>
        <p>{{ showMinTemp }}</p>
        <p>{{ showMaxTemp }}</p>
      </div>
    </div>

  </main>
</template>

<style scoped>
  .wrapper {
    width: 900px;
    height: 500px;
    border-radius: 50px;
    padding: 20px;
    background: #383131;
    text-align: center;
    color: #fff;
  }
  .wrapper h1 {
    margin-top: 50px;
  }
  .wrapper p {
    margin-top: 20px;
  }
  .wrapper input {
    margin-top: 30px;
    background-color: transparent;
    border: 0;
    border-bottom:2px solid #0e0e0e;
    color: #fff;
    font-size: 14px;
    padding: 5px 8px;
    outline: none;
  }
  .wrapper input:focus {
    border-bottom-color: #6e2d7d;
  }

  .wrapper button:disabled {
    background-color: #9b43ae;
    cursor: not-allowed;
  }

  .wrapper button {
    background-color: #e33bc4;
    color: #fff;
    border-radius: 10px;
    border: 2px solid #b999;
    padding: 10px 15px;
    margin-left: 20px;
    cursor: pointer;
    transition: transform 500ms ease;
  }
  .wrapper button:hover {
    transform: scale(1.1), translateY(-5px)
  }

  .error {
    color: rgb(185, 23, 23);
  }
</style>
