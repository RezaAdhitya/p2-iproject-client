<script>
import { mapActions, mapState, mapWritableState } from 'pinia';
import { useMainStore } from '../stores/main';

export default {
  computed: {
    ...mapState(useMainStore, ['loggedIn', 'spotifyUser', 'currentWeather', 'weatherInfo', 'isGuest']),
    getWeatherInfo() {
      if (this.currentWeather?.weather[0].description.includes('clear')) {
        return this.weatherInfo.clear.displayName
      } else if (this.currentWeather?.weather[0].description.includes('cloud')) {
        return this.weatherInfo.cloud.displayName
      } else if (['fog', 'haze', 'mist'].some(el => this.currentWeather?.weather[0].description.includes(el))) {
        return this.weatherInfo.fog.displayName
      } else if (this.currentWeather?.weather[0].description.includes('rain')) {
        return this.weatherInfo.rain.displayName
      } else if (this.currentWeather?.weather[0].description.includes('snow')) {
        return this.weatherInfo.snow.displayName
      } else if (this.currentWeather?.weather[0].description.includes('storm')) {
        return this.weatherInfo.storm.displayName
      } else if (this.currentWeather?.weather[0].description.includes('sun')) {
        return this.weatherInfo.sun.displayName
      } else {
        return 'Unknown'
      }
    },

    getLogoInfo() {
      if (this.currentWeather?.weather[0].description.includes('clear')) {
        return this.weatherInfo.clear.logo
      } else if (this.currentWeather?.weather[0].description.includes('cloud')) {
        return this.weatherInfo.cloud.logo
      } else if (['fog', 'haze', 'mist'].some(el => this.currentWeather?.weather[0].description.includes(el))) {
        return this.weatherInfo.fog.logo
      } else if (this.currentWeather?.weather[0].description.includes('rain')) {
        return this.weatherInfo.rain.logo
      } else if (this.currentWeather?.weather[0].description.includes('snow')) {
        return this.weatherInfo.snow.logo
      } else if (this.currentWeather?.weather[0].description.includes('storm')) {
        return this.weatherInfo.storm.logo
      } else if (this.currentWeather?.weather[0].description.includes('sun')) {
        return this.weatherInfo.sun.logo
      } else {
        return 'src/assets/images/warning-2-128.png'
      }
    }
  },
  methods: {
    ...mapActions(useMainStore, ['doLogout',]),
    logout() {
      this.doLogout()
    },

  }
}

</script>

<template>
  <div class="navbar h-[5vh] bg-transparent flex justify-end items-center px-8 pt-4 gap-4">
    <div v-if="Object.keys(currentWeather).length !== 0" class="weather-info flex justify-end items-center h-full">
      <a @click.prevent="$router.push('/')" class="right text-[1.3rem] pt-4 pr-2 font-bold text-theme_red" href="#">
        {{ getWeatherInfo }}, {{ this.currentWeather.temp }}°C
      </a>
      <img :src="`/images/${getLogoInfo}`" class="w-auto h-[30px] mt-4 border-r-2 border-theme_red border-solid pr-4" />
    </div>
    <div v-if="spotifyUser" class="weather-info flex justify-end items-center h-full">
      <p class="right text-[1.3rem] pt-4 pr-2 font-bold text-green-500" href="#">
        Spotify User
      </p>
      <img src="/images/spotify-user.png" class="w-auto h-[30px] mt-4 border-r-2 border-theme_red border-solid pr-4" />
    </div>
    <div v-if="isGuest" class="weather-info flex justify-end items-center h-full">
      <p class="right text-[1.3rem] pt-4 pr-2 font-bold text-blue-500" href="#">
        Guest User
      </p>
      <div class="w-auto h-[30px] mt-4 border-r-2 border-theme_red border-solid pr-4" />
      <!-- <img src="/images/spotify-user.png" class="w-auto h-[30px] mt-4 border-r-2 border-theme_red border-solid pr-4" /> -->
    </div>
    <a @click.prevent="$router.push('/')"
      class="ml-4right text-[1.3rem] text-center w-[4rem] pt-4 font-bold text-theme_red hover:text-[1.5rem] transition-all"
      href="#">Home</a>
    <a @click.prevent="logout"
      class="right text-[1.3rem] pt-4 font-bold text-center w-[4rem] text-theme_red hover:text-[1.5rem] transition-all"
      href="#">Logout</a>
  </div>
</template>