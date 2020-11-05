<template>
  <q-page class="flex colimn">
    <div class="col q-pt-lg q-px-md">
          <q-input  
          v-model="search"
          @keyup.enter="getWeatherBySearch"
          placeholder="Search"
          dark
          borderless 
          >
        <template v-slot:before>
          <q-icon 
            @click="getLocation"
            name="my_location" />
        </template>

        <template v-slot:append>
          <q-btn
          @click="getWeatherBySearch"
          round dense flat icon="search" />
        </template>
      </q-input>
    </div>
    <template v-if="weatherData">
    <div class="col text-white text-center">
      <div class="text-h4 text-weight-light">
        {{weatherData.name}}
      </div>
      <div class="text-h6 text-weight-light">
        {{weatherData.weather[0].main}}
      </div>
      <div class="text-h1 text-weight-thin
      q-my-lg relative-position">
        <span>{{Math.round(weatherData.main.temp)}}</span>
        <span>&deg;c</span>
      </div>
    </div>
    </template>
    <!-- <div class="col text-center">
      <img src="https://www.fillmurray.com/100/100" alt="Conner">
    </div> -->
    <template v-else>
      <div class="col column text-center text-white">
      <div class="col text-h2 text-weight-thin">
        Quasar<br>Weather
      </div>
      <q-btn
        @click="getLocation"
        class="col" 
        flat>
        <q-icon left size="3em" 
        name="my_location" />
        <div>Find my location</div>
      </q-btn>
      </div>
    </template>
  </q-page>
</template>

<script>
export default {
  name: 'PageIndex',
  data(){
    return{
      search:'',
      weatherData:null,
      lat:null,
      lon:null,
      apiUrl:'https://api.openweathermap.org/data/2.5/weather',
      apiKey:'92d9164537db7b1a73dc326da9a8a975'
    }
  },
  methods:{
    getLocation(){
      navigator.geolocation.getCurrentPosition
      (position =>{
        console.log('position:', position)
        this.lat = position.coords.latitude
        this.lon = position.coords.longitude
        this.getWeatherByCoords()
      })
    },
    getWeatherByCoords(){
      this.$axios(`${this.apiUrl}?lat=${this.lat}&lon=${this.lon}&appid=${this.apiKey}$units=metric`).then(response =>{
        console.log('response:', response)
        // this.weatherData=response.data
      })//'${this.apiUrl}?lat=${this.lat)&lon=${this.lon}&appid=${this.apiKey}$units=metric'
    },
    getWeatherBySearch(){
      this.$axios(`${this.apiUrl}?q=${this.search}&appid=${this.apiKey}&units=metric`).then(response =>{
        this.weatherData = response.data
      })
    },
  }
}
</script>

<style lang="sass">
  .q-page
    background: linear-gradient(to top, 
    #556270, #4ECDC4)
  .degree
    top: -44px
</style>