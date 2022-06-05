<template>
    <div class="weather-app">
        <img class="bg-img" src="../assets/bg.jpeg" alt="">
        <div class="weather-wrap">
        <h1 class="fw-bold">New York</h1>

        <div class="weather-temp py-4">{{weather.temp}} °C</div>
        <div class="d-flex justify-content-between align-items-center col-12">
            <div>{{format_date(weather.time)}}</div>
            <div class="d-flex hstack gap-3">
                <div class="high-temp d-flex">
                <img class="icon pe-1" src="../assets/weather-elements/high-temperature.png" alt="">
                <div>{{daily.tempmax[0]}}</div>
                </div>
                <div class="high-temp d-flex">
                <img class="icon pe-1" src="../assets/weather-elements/low-temperature.png" alt="">
                <div>{{daily.tempmin[0]}}</div>
                </div>
            </div>
        </div>
        

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
      daily: {
          tempmax: '0',
          tempmin: '0',
      },
    }
  },
  methods: {
      format_date(value){
         if (value) {
           return moment(String(value)).format('ddd DD MMM, LT')
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
    },
  },

  created(){
    this.getWeather();
  },

}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;700&display=swap');
*,
*::before,
*::after {
  box-sizing: border-box;
}
body {
  padding: 0;
  margin: 0;
  font-family: 'Poppins', sans-serif;
  
}
.weather-app{
    position: relative;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    height: 100vh;
}
.icon{
    width: 25px;
}
.bg-img{
    position: absolute;
    background-position: center;
    width: 100%;
    height: 100vh;
    object-fit: cover;
    filter: brightness(50%);
}
.weather-wrap{
    position: absolute;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    width: 50%;
    background-color: rgba(250, 235, 215, 0.9);
    padding: 12px 20px;
    border-radius: 25px;
    box-shadow: rgba(17, 12, 46, 0.15) 0px 48px 100px 0px;
}
.weather-wrap .weather-temp{
    font-size: 30px;
}
</style>