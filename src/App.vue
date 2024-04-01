<script setup>
import { ref } from 'vue';
const apiKey = ref("56e036b3baf17597fefbf93c6a941d39");
const apiUrl = ref("https://api.openweathermap.org/data/2.5/weather?units=metric&q=");
const city = ref("");
const weather = ref(null);
const error = ref(null);

const fetchWeather = async () => {
    try {
        const response = await fetch(`${apiUrl.value}${city.value}&appid=${apiKey.value}`);
        const data = await response.json();
        console.log(data);
        console.log(data.name)
        if (data.cod == "404") {
            error.value = "Weather not found";
            weather.value = null;
        } else {
            error.value = null;
            weather.value = data;
        }
    } catch (err) {
        console.error("Error fetching data:", err);
        error.value = "An error occurred while fetching data";
        weather.value = null;

    }
};
</script>

<template>
    <div class="container">
        <div class="search">
            <input v-model="city" type="text" placeholder="Enter your city name">
            <button @click="fetchWeather">Search</button>
        </div>
        <div class="error" v-if="error">
            {{ error }}
        </div>
        <div class="weather" v-if="weather">
            <h2 class="city">{{ weather.name }}</h2>
            <div class="temp">{{ weather.main.temp }}°C</div>
            <div class="humidity">{{ weather.main.humidity }}%</div>
            <div class="wind">{{ weather.wind.speed }} km/h</div><br>
        </div>
    </div>
</template>
