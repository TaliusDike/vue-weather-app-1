<template>
  <div class="container">
    <div class="inner-container">
      
      <h2>{{ temperature }} <span>&#176;</span> </h2>
      <p>{{ weatherDescription }}</p>

      <div class="input-container">
        <label for="latitude">Latitude</label>
        <input type="text" name="latitude" v-model="latitude">
      </div>

      <div class="input-container">
        <label for="longitude">Longitude</label>
        <input type="text" name="longitude" v-model="longitude">
      </div>

      <button @click="GetWeatherData()">Get Data</button>

      <div v-if="showHaze" class="weather-condition">
        <img src="../assets/haze.png" alt="" />
      </div>

      <div v-if="showRainy" class="weather-condition">
        <img src="../assets/rainy.png" alt="" />
      </div>

      <div v-if="showSunny" class="weather-condition">
        <img src="../assets/sunny.png" alt="" />
      </div>

      <div v-if="showCloudy" class="weather-condition">
        <img src="../assets/cloudy.png" alt="" />
      </div>

    

    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      weather: {},
      weatherDescription: "",
      showCloudy: false,
      showRainy: false,
      showSunny: false,
      showHaze: false,
      temperature: "",
      latitude: '6.5244',
      longitude: '3.3792'
    };
  },

  methods: {
    GetWeatherData() {
      axios
        .get(
          "https://api.openweathermap.org/data/2.5/weather?lat=6.5244&lon=3.3792&appid=76115a6c756e0de1533e5ffeefd2f6cd&units=metric"
        )
        .then((response) => {
          console.log(response.data.weather[0]);
          let mainDescription = response.data.weather[0].main;
          let descriptionString = response.data.weather[0].description;
          this.weatherDescription = descriptionString.charAt(0).toUpperCase() + descriptionString.slice(1);
          // this.weatherDescription = response.data.weather[0].description;
          this.temperature = response.data.main.temp;

          if(mainDescription == 'Haze') {
            this.showHaze = true;
          }

          if (mainDescription == 'Sunny') {
            this.showSunny = true;
          }

          if (mainDescription == 'Clouds') {
            this.showCloudy = true;
          }

          if (mainDescription == 'Clear') {
            this.showRainy = true;
          }


        })
        .catch((error) => {
          console.log(error);
        });
    },
  },

  mounted () {
    this.GetWeatherData();
  }

};
</script>

<style>
.container {
  width: 100%;
  display: flex;
  justify-content: center;
  background-color: pink;
}

.inner-container {
  width: 50%;
  background-color: blue;
}
</style>
