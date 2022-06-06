<template>
    <div class="weather-app">
        <img class="bg-img" src="../assets/bg.jpeg" alt="">
        <div class="weather-wrap">
        <h1 class="fw-bold text-center">New York</h1>

        <div class="weather-temp fs-1 text-center my-4">{{weather.temp}} °C</div>
        <div class="d-flex justify-content-between align-items-center col-12">
            <div>{{format_full_date(weather.time)}}</div>
            <div class="d-flex hstack gap-3">
                <div class="high-temp d-flex">
                <img class="icon pe-1" src="../assets/weather-elements/high-temperature.png" alt="">
                <div>{{Math.round(daily.tempmax[0])}}</div>
                </div>
                <div class="high-temp d-flex">
                <img class="icon pe-1" src="../assets/weather-elements/low-temperature.png" alt="">
                <div>{{Math.round(daily.tempmin[0])}}</div>
                </div>
            </div>
        </div>
        
        <!--hourly temperature-->
        <div class="fw-bold mt-4 text-start">Hourly temperature</div>    
        <div class="hourly-temp d-flex justify-content-between align-items-baseline col-12 mt-1">
            <div class="d-flex flex-column flex-start ps-5 py-3">
                <div class="py-2 fw-bold" v-for="time in hourly.time.slice(0, 24)" v-bind:key="time.id">
                        {{format_hour_date(time)}} h
                </div>
            </div>    
            <div class="d-flex flex-column pe-5">
                <div class="py-2" v-for="temp in hourly.temp.slice(0, 24)" v-bind:key="temp.id">
                        {{Math.round(temp)}} °C
                </div>
            </div>
        </div>

        <!--<div class="hour d-flex flex-column align-items-center">
        <div class="hourly-temp d-flex flex-row col-12 hstack gap-3">
            <div v-for="time in hourly.time.slice(0, 24)" v-bind:key="time.id">      
                <div>
                    {{format_hour_date(time)}}
                </div>
            </div>
        </div>
        <div class="hourly-temp d-flex flex-row col-12 hstack gap-3">
            <div v-for="temp in hourly.temp.slice(0, 24)" v-bind:key="temp.id">      
                <div>
                    {{Math.round(temp)}}
                </div>
            </div>
        </div>
        </div>-->


        <p>{{daily.tempmax[0]}}</p>
        <p>{{daily.tempmax[1]}}</p>
        <p>{{daily.tempmax[2]}}</p>
        <ul v-for="max in tempmax" :key="max">
        <li>{{max}}</li>
        </ul>

        <!--<button @click="getWeather()">click</button>-->
        </div>
    </div>
</template>

<script>
import moment from 'moment'

export default {
    name : 'Weather',
    data () {
    return {
      weather: {
        windspeed: '',
        winddirection: '',        
        temp: '',
        time: '',
        weathercode: '',
      },
      hourly: {
        time: '',
        temp: '',
      },
      daily: {
          tempmax: '0',
          tempmin: '0',
      },
    }
  },
  methods: {
    format_full_date(value){
        if (value) {
        return moment(String(value)).format('ddd DD MMM, LT')
        }
    },
    format_hour_date(value){
        if (value) {
        return moment(String(value)).format('HH')
        }
    },
    getWeather: async function () {
      const baseurl = 'https://api.open-meteo.com/v1/forecast?latitude=40.71&longitude=-74.01&hourly=temperature_2m&daily=temperature_2m_max,temperature_2m_min,sunrise,sunset&current_weather=true&timezone=America%2FNew_York'
      const res = await fetch(baseurl)
      const results = await res.json()

      console.log(results)

      this.weather.temp = Math.round(results.current_weather.temperature)
      this.weather.time = results.current_weather.time

      this.daily.tempmax = results.daily.temperature_2m_max
      this.daily.tempmin = results.daily.temperature_2m_min

      this.hourly.time = results.hourly.time
      this.hourly.temp = results.hourly.temperature_2m
    },
  },

  created(){
    this.getWeather();
  },

}
</script>

<style>
@import "../assets/css/style.css";
</style>