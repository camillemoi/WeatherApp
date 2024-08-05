<template>
  <div
    :class="
      isNightTime()
        ? { 'weather-container bck-night': true }
        : { 'weather-container bck-day': true }
    "
  >
    <div class="weather-wrap">
      <div class="search-box">
        <input
          type="text"
          placeholder="Search..."
          class="search-bar"
          v-model="query"
          v-on:keypress="fetchWeather"
        />
      </div>
      <div class="weather-info" v-if="weather.location.name != ''">
        <div class="location-box">
          <div class="location">{{ weather.location.name }},{{ weather.location.country }}</div>
          <div class="date">{{ todaysDate() }}</div>
        </div>
        <div class="weather-box">
          <div class="temp" v-if="activeDegree === 'c'">{{ weather.current.temp_c }}°c</div>
          <div class="temp" v-else>{{ weather.current.temp_f }}°f</div>
          <img
            :class="isNightTime() ? { 'icon-night': true } : { 'icon-day': true }"
            :src="getIcon()"
          />
          <div class="weather">{{ weather.current.condition.text }}</div>
        </div>
        <div class="switch-container">
          <div class="temperature-container">
            <span class="switch-temp">°C</span>
            <label class="switch">
              <input type="checkbox" @change="switchTemp()" />
              <span class="slider round"></span>
            </label>
            <span class="switch-temp">°F</span>
          </div>
          <div class="forecast">
            <span>Prévisions</span>
            <button class="more-btn">+</button>
          </div>
        </div>
      </div>
      <div v-else>
        <div class="location-box">
          <div class="weather-init">Search for a location</div>
          <img class="icon-home" src="../assets/main_icon.png" />
        </div>
      </div>
    </div>
  </div>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@300;500;700;900&display=swap');
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Montserrat';
}
.weather-container {
  background-size: cover;
  background-position: center;
  transition: 0.4s;
  width: 375px;
  margin: 0 auto;
  border-radius: 25px;
  margin-top: 50px;
  box-shadow: 0px 0px 30px #00000065;
}
.bck-day {
  background-image: url('../assets/day.jpg');
}
.bck-night {
  background-image: url('../assets/night.jpg');
}
.weather-wrap {
  height: 600px;
  padding: 25px;
  border-radius: 25px;
}
.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;
  color: #313131;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  background: none;
  background-color: rgba(255, 255, 255, 0.5);
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  border-radius: 10px;
  transition: 0.4s;
}
.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
}
.location-box .location {
  color: white;
  font-size: 20px;
  font-weight: 500;
  font-style: italic;
  text-align: center;
  margin-top: 30px;
}
.location-box .date {
  color: white;
  font-size: 15px;
  font-weight: 300;
  text-align: center;
}
.weather-box {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 35px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(206, 206, 206, 0.25);
  border-radius: 16px;
  margin: 20px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  font-style: italic;
}
.weather-box .weather {
  color: #fff;
  font-size: 25px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 3px rgba(0, 0, 0, 0.25);
}
.weather-init {
  color: white;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  margin-top: 30px;
}
.icon-home {
  width: 200px;
  margin-left: 80px;
  margin-top: 20px;
}
.icon-day {
  width: 220px;
}
.icon-night {
  width: 150px;
  margin-top: 20px;
}
.switch {
  position: relative;
  display: inline-block;
  width: 60px;
  height: 34px;
}
.switch input {
  opacity: 0;
  width: 0;
  height: 0;
}
.slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #ccc;
  -webkit-transition: 0.4s;
  transition: 0.4s;
}
.slider:before {
  position: absolute;
  content: '';
  height: 26px;
  width: 26px;
  left: 4px;
  bottom: 4px;
  background-color: white;
  -webkit-transition: 0.4s;
  transition: 0.4s;
}
input:checked + .slider {
  background-color: #2196f3;
}
input:focus + .slider {
  box-shadow: 0 0 1px #2196f3;
}
input:checked + .slider:before {
  -webkit-transform: translateX(26px);
  -ms-transform: translateX(26px);
  transform: translateX(26px);
}
.slider.round {
  border-radius: 34px;
}
.slider.round:before {
  border-radius: 50%;
}
.switch-temp {
  color: #686666;
  font-size: 18px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 1px white;
}
.switch-container {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-top: 22px;
}
.more-btn {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 26px;
  width: 26px;
  background-color: white;
  border-radius: 34px;
  border: 1px solid #686666;
  font-size: 32px;
  cursor: pointer;
}
.temperature-container {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 10px;
}
.forecast {
  display: flex;
  flex-direction: row;
  justify-content: center;
  gap: 7px;
  text-shadow: 1px 1px white;
}
</style>
<script lang="ts">
import axios from 'axios'
import { weatherEvents } from './models/weather-classifier.ts'
export default {
  data() {
    return {
      api_key: 'd49255987f5d4c9882272448240308',
      url_base: 'http://api.weatherapi.com/v1/current.json?',
      weather_icon: '../assets/main_icon.png',
      query: '',
      weather: {
        current: {
          temp_c: 0,
          temp_f: null,
          condition: {
            text: '',
            icon: '',
            code: 0
          }
        },
        location: {
          name: '',
          country: ''
        }
      },
      activeDegree: 'c'
    }
  },
  methods: {
    async fetchWeather(e: any) {
      if (e.key == 'Enter') {
        let response = await axios.get(`${this.url_base}key=${this.api_key}&q=${this.query}&aqi=no`)
        // let mock = {
        //data: {
        //location: { name: 'London', country: 'United Kingdom' },
        //current: { temp_c: 18, temp_f: 70, condition: { text: 'Light drizzle', code: 1063 } }
        //}
        //}
        this.setResults(response.data)
      }
    },
    setResults(returnedResponse: any) {
      this.getIcon()
      this.weather = returnedResponse
    },
    todaysDate() {
      const months = [
        'January',
        'February',
        'March',
        'April',
        'May',
        'June',
        'July',
        'August',
        'September',
        'October',
        'November',
        'December'
      ]
      const days = ['Sunday', 'Monday', 'Tueday', 'Wednesday', 'Thursday', 'Friday', 'Saturday']
      let d = new Date()
      let month = months[d.getMonth()]
      let day = days[d.getDay()]
      let date = d.getDate()
      let year = d.getFullYear()
      return `${day} ${date} ${month} ${year}`
    },
    getIcon() {
      let events = this.isNightTime() ? weatherEvents.night : weatherEvents.day
      const icon = events.find((event) =>
        event.values.includes(this.weather.current.condition.code)
      )?.icon

      if (icon) return new URL(icon, import.meta.url).href
    },
    isNightTime() {
      const d = new Date()
      const time = d.getHours()
      const nightTime = [21, 22, 23, 24, 0, 1, 2, 3, 4, 5]
      return nightTime.includes(time)
    },
    switchTemp() {
      this.activeDegree = this.activeDegree === 'c' ? 'f' : 'c'
    }
  }
}
</script>
