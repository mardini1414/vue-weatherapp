<template>
  <div class="sm:w-[400px]">
    <div class="px-2 py-4 mb-4 bg-white rounded-md backdrop-blur-md" v-if="notFound">
      <span class="text-red-500">Oppss City not found!</span>
    </div>
    <div class="text-center">
      <span class="text-white">{{ city }} {{ countryId }}</span>
      <span class="ml-4 text-white">{{ weather }}</span>
    </div>
    <div class="flex items-center justify-center">
      <span class="text-6xl font-thin text-white">{{ Math.round(temp) }}&deg;C</span>
      <img :src="`http://openweathermap.org/img/wn/${image}@2x.png`" alt="test" />
    </div>
    <form @submit.prevent @submit="getData">
      <input
        type="text"
        v-model="search"
        class="w-full px-2 py-1 text-white bg-transparent border border-white rounded-lg outline-none focus:border-2"
        placeholder="Enter your city"
      />
    </form>
    <div class="flex justify-center gap-8 mt-4">
      <div class="text-center">
        <small class="block text-white/95">Humidity</small>
        <strong class="text-lg text-white">{{ humidity }}%</strong>
      </div>
      <div class="text-center">
        <small class="block text-white/95">Visibility</small>
        <strong class="block text-lg text-white">{{ visibility / 1000 }} km</strong>
      </div>
      <div class="text-center">
        <small class="block text-white/95">Wind speed</small>
        <strong class="block text-lg text-white">{{ windSpeed }} m/h</strong>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      city: "Bekasi",
      countryId: "",
      temp: 0,
      weather: "",
      image: "",
      humidity: 0,
      visibility: 0,
      windSpeed: 0,
      search: "",
      notFound: false,
    };
  },
  methods: {
    async getData() {
      try {
        const result = await axios.get(
          `https://api.openweathermap.org/data/2.5/weather?q=${
            this.search || this.city
          }&appid=cacb232f4b748d0760b2ba579000d0d8&units=metric`
        );
        this.city = result.data.name;
        this.countryId = result.data.sys.country;
        this.weather = result.data.weather[0].description;
        this.temp = result.data.main.temp;
        this.image = result.data.weather[0].icon;
        this.humidity = result.data.main.humidity;
        this.visibility = result.data.visibility;
        this.windSpeed = result.data.wind.speed;
        this.notFound = false;
      } catch (error) {
        if (error.response.status === 404) {
          this.notFound = true;
        } else {
          this.notFound = false;
        }
      }
    },
  },

  mounted() {
    this.getData();
  },
};
</script>
