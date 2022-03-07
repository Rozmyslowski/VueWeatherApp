<template>
  <div id="main" >
      <div class="card-nav">
        <h1 class="title">Weather App</h1>

         <form class="search-location" v-on:submit.prevent="getWeather">
            <input
            type="text"
            class="form-control text-muted form-rounded p-4 shadow-sm"
            placeholder="Wpisz Miasto?"
            v-model="citySearch"
            autocomplete="off"
          />
        </form>
        <p class="text-center my-3" v-if="cityFound">Nie ma takiego miasta!</p>

        <div class="card-top text-center" style="margin-bottom: 15rem">
            <div class="city-name my-3">
              <p><img src="./assets/geo.svg" alt=""> {{ weather.cityName}} • {{ weather.country }}</p>
            </div>
        </div>
     </div>

    <div  style=" margin: 0 auto; width: 80%; height: 900px;">
        <div class="card-box-three">
          <div class="row">
              <div class="col-12 text-center temp">
                <span>{{ weather.temperature }}&deg;C</span>
                <p class="my-4">{{ weather.description }}</p>
              </div>
            </div>
          </div>  


        <div class="card-box-five">
            <div class="">
                <p>
                 MIN. {{ weather.lowTemp }}&deg;C
                </p>
                <p>
                  MAX. {{ weather.highTemp }}&deg;C
                </p>
              </div>
            </div>
        </div>
           
        <div class="card-box-nine">
            <div class="col text-center">
              <span>Pressure</span>
              <p>{{ weather.pressure }}hPa</p>
            </div>
          </div>

         
          <div class="card-box-one">
            <div class="col text-center">
              <span>Feels Like</span>
              <p>{{ weather.feelsLike }}&deg;C</p>
            </div>
          </div>
          
          <div class="card-box-two">
          <div class="col text-center">
            <p>{{ weather.humidity }}%</p>
              <span>Humidity</span>
            </div>
           </div>




         <div class="future-forecast" id="weather-forecast">
            <div class="weather-forecast-item">
              <div class="day">tue</div>
              <img src="" alt="weather icon" class="w-icon">
              <div class="temp">21 &deg;C</div>
            </div>

            <div class="weather-forecast-item">
              <div class="day">tue</div>
              <img src="" alt="weather icon" class="w-icon">
              <div class="temp">21 &deg;C</div>
            </div>

            <div class="weather-forecast-item">
              <div class="day">tue</div>
              <img src="" alt="weather icon" class="w-icon">
              <div class="temp">21 &deg;C</div>
            </div>

          </div>
         
  </div>
</template>

<script>
export default {
  data() {
    return {
      cityFound: false,
      visible: false,
      stormy: false,
      cloudy: false,
      clearSky: false,
      clearNight: false,
      snowy: false,
      isDay: true,
      citySearch: "",

      
      weather: {
        cityName: "",
        country: "",
        temperature: '-',
        description: "-",
        lowTemp: "-",
        highTemp: "-",
        feelsLike: "-",
        humidity: "-",
        pressure: "-",
      
      },
    };
  },
  methods: {
    getWeather: async function () {
      console.log(this.citySearch);
      const key = '40ad7bff961e06f4ff202636ea1fd452'
      const baseURL = `https://api.openweathermap.org/data/2.5/weather?q=${this.citySearch}&appid=${key}&units=metric`;
      
      try {
        const response = await fetch(baseURL);
        const data = await response.json();
        console.log(data);
        this.citySearch = "";
        this.weather.cityName = data.name;
        this.weather.country = data.sys.country;
        this.weather.temperature = Math.round(data.main.temp);
        this.weather.description = data.weather[0].description;
        this.weather.lowTemp = Math.round(data.main.temp_min);
        this.weather.highTemp = Math.round(data.main.temp_max);
        this.weather.feelsLike = Math.round(data.main.feels_like);
        this.weather.humidity = Math.round(data.main.humidity);
        this.weather.pressure = Math.round(data.main.pressure)
        

        const timeOfDay = data.weather[0].icon;
        if (timeOfDay.includes("n")) {
          this.isDay = false;
        } else {
          this.isDay = true;
        }
       
        this.visible = true;
        this.cityFound = false;
      } catch (error) {
        console.log(error);
        this.cityFound = true;
        this.visible = false;
      }
    },
  },
};
</script>

<style scoped>
@import "./assets/custom.css";
</style>