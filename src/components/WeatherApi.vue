<script setup lang="ts">
import { computed, ref } from "vue";
import { WeatherApp } from "../models/WeatherInterface";
import axios from "axios";

const APIKEY = import.meta.env.VITE_API_KEY;

const city = ref("");
const weather = ref<WeatherApp>();

//API anropet via axios
const fetchWeather = async () => {
  try {
    const response = await axios.get<WeatherApp>(
      `https://api.openweathermap.org/data/2.5/weather?units=metric&q=${city.value}&appid=${APIKEY}`
    );
    weather.value = response.data;
    console.log(response.data);
  } catch (e) {
    console.error("Could not retrieve weather");
  }
};
const mathRounded = computed(() => {
  return weather.value ? Math.round(weather.value.main.temp) : null;
});
</script>

<template>
  <div>
    <h2 class="poppinsFont">Weather App</h2>
    <form @submit.prevent="fetchWeather">
        <div class="inputButtonContainer">
      <input class="inputField" type="text" v-model="city" placeholder="Search for city ..." />
      <button class="searchButton">Search</button>
    </div>
    </form>
    <div class="weatherResultContainer" v-if="weather">
      <h2 class="poppinsFont">{{ weather.name }}</h2>
      <img
        :src="`http://openweathermap.org/img/wn/${weather.weather[0].icon}@2x.png`"
        alt="Weather icon"
      />
      <p class="celsiusText">{{ mathRounded }}°C</p>
      <!-- <p>{{ weather.weather[0].description }}</p> -->
    </div>
  </div>
</template>

<style scoped>
.weatherResultContainer {
  display: flex;
  align-items: center;
  flex-direction: column;
}

.inputField {
    color: black;
    background-color: transparent;
    border: none;
    border-bottom: 1px solid gray;
    padding: 5px;
    font-size: 16px;
    letter-spacing: 1px;
    font-family: "Poppins", sans-serif;
    font-weight: 500;
    font-style: normal;
}

.inputField:focus {
    border: none;
    border-bottom: 1px solid gray;
    outline: none;
}

img {
  width: 50px;
}

h2, p {
    color: black;
}

.searchButton {
    color: black;
    background-color: transparent;
    border: 1px solid rgba(3, 3, 3, 0.726);
    padding: 5px 10px;
    font-size: 0.9rem;
    max-width: 70px;
    font-family: "Poppins", sans-serif;
    font-weight: 500;
    font-style: normal;
    margin-bottom: 2rem;
}

.searchButton:hover {
    background-color: rgb(51, 51, 51);
    color: aliceblue;
    transition: 0.5s;
}

.inputButtonContainer {
    display: flex;
    flex-direction: column;
    gap: 1em;
    align-items: center;
}

.celsiusText, .poppinsFont {
    font-family: "Poppins", sans-serif;
    font-weight: 500;
    font-style: normal;
    letter-spacing: 1px;
}

.weatherResultContainer {
    border: 1px solid rgba(128, 128, 128, 0.459);
    padding: 40px;
    border-radius: 10px;
    box-shadow: 2px 3px 10px #707070;
}

.weatherResultContainer:hover {
    transition: 0.5s;
    background-color: rgba(5, 5, 5, 0.808);
}

.weatherResultContainer:hover h2,
.weatherResultContainer:hover p {
    color: rgb(248, 231, 176);
    transition: color 0.5s;
}

</style>
