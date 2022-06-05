<template>
    <div class="weather-app">
        <img src="../assets/bg.jpeg" alt="">
        <div class="weather-wrap">
        <h1>New York</h1>

        <h1>{{weather.temp}} °C</h1>

        <p>{{daily.tempmax[0]}}</p>
        <p>{{daily.tempmax[1]}}</p>
        <p>{{daily.tempmax[2]}}</p>
        <ul v-for="max in tempmax" :key="max">
        <li>{{max}}</li>
        </ul>
        <!--<div class="daysDetail">
        <span
          style="display:flex; flex-direction:column; align-items:center; padding-left:10px"
          v-for="data in seven.list"
          :key="data">
        <p>
          {{data.day}}
        </p>
          <p>{{ data.weather[0].main }}</p>
          <p>{{ parseInt(data.temp.day) }}° / {{ parseInt(data.temp.night) }}°</p>
        </span>
      </div>-->

        <!--<button @click="getWeather()">click</button>-->
        </div>
    </div>
</template>

<script>
export default {
    name : 'Weather',
    props: ['daily', 'seven'],
    data () {
    return {
      weather: {
        windspeed: '',
        winddirection: '',        
        temp: '',
        weathercode: '',
      },
      daily: {
          tempmax: '',
          tempmin: '',
      },
      days: ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'],
      daysIndex: [],
    }
  },
  methods: {
    getWeather: async function () {
      const baseurl = 'https://api.open-meteo.com/v1/forecast?latitude=40.71&longitude=-74.01&hourly=temperature_2m&daily=temperature_2m_max,temperature_2m_min,sunrise,sunset&current_weather=true&timezone=America%2FNew_York'
      const res = await fetch(baseurl)
      const results = await res.json()

      console.log(results)

      this.results = results
      this.weather.temp = results.current_weather.temperature
      this.latitude = results.latitude
      this.longitude = results.longitude
      this.elevation = results.elevation

      this.daily.tempmax = results.daily.temperature_2m_max
    },
  },

  created(){
    this.getWeather();

    var currentDate = new Date();
    var nextWeek = new Date(currentDate.getTime() + 7 * 24 * 60 * 60 * 1000);
    var days = []
    while (currentDate <= nextWeek) {
      days.push(new Date(currentDate).getDay());
      currentDate.setDate(currentDate.getDate() + 1);
    }
    this.daysIndex = days.slice(1);
  },

  /*watch:{
  seven:function(){
     for (let i = 0; i < this.daysIndex.length; i++) {
       this.seven.list[i].day = this.days[this.daysIndex[i]]
    }
  }
}*/

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
img{
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
</style>