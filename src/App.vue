<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 27 ? 'warm': ''">
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Search..."
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country}}</div>
          <div class="date">{{ getToday() }}</div>
        </div>
      </div>

      <div class="weather-box" v-if="typeof weather.main != 'undefined'">
        <div class="temp">{{Math.round(weather.main.temp)}}°c</div>
        <div class="weather">{{ weather.weather[0].main }}</div>
        <div class="weather-description">{{ weather.weather[0].description }}</div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      api_key: process.env.VUE_APP_WEATHER_API_KEY,
      url: process.env.VUE_APP_BACKEND_URL,
      query: "",
      weather: {}
    };
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.url}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
        )
          .then(data => {
            return data.json();
          })
          .then(this.setResults);
      }
    },

    setResults(result) {
      console.log(result);
      this.weather = result;
    },

    getToday() {
      let d = new Date();
      let months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December"
      ];
      let days = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday"
      ];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day}, ${date} ${month} ${year}`;
    }
  }
};
</script>