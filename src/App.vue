<template>
  <div
    id="app"
    :class="
      typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''
    "
  >
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Tìm kiếm...."
          v-model="query"
          @keyup.enter="fetchWeather"
        />
      </div>
      <div class="weather-wrap" v-if="weather.main != ''">
        <div class="location-box">
          <h3 class="location">{{ weather.name }}, {{ country }}</h3>
          <p class="date">{{ formatDate() }}</p>
        </div>
        <div class="desc">
          <p class="temp">{{ Math.round(temp) || "" }}°c</p>
          <p class="weather">{{ weatherMain }}</p>
        </div>
      </div>
    </main>
  </div>
</template>
<script>
export default {
  name: "app",
  data() {
    return {
      api_key: "9abe6d260c17e0143c7947428518da23",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
      country: "",
      temp: "",
      weatherMain: "",
    };
  },
  methods: {
    async fetchWeather() {
      await fetch(
        `${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
      )
        .then((res) => res.json())
        .then(this.getData);
    },
    getData(data) {
      if (data != undefined) {
        this.weather = data;
        this.country = data.sys.country;
        this.weatherMain = data.weather[0].main;
        this.temp = Math.round(data.main.temp);
      }
    },
    formatDate() {
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
        "December",
      ];
      let days = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`;
    },
  },
};
</script>

<style>
@import "./assets/base.css";
#app {
  background-image: url("./assets/cold-bg.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}
#app.warm {
  background-image: url("./assets/warm-bg.jpg");
}
main {
  margin:0 auto;
  max-width:800px;
  min-height: 100vh;
  padding: 20px;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );
}
.search-box {
  width: 100%;
  margin-top: 30px;
}
.search-bar {
  display: block;
  width: 100%;
  padding: 15px;

  outline: none;
  border: none;
  font-size: 20px;
  color: #313131;
  background-color: rgba(0, 0, 0, 0.25);
  transition: 0.4s;
  border-radius: 0 16px 0 16px;
}
.search-bar:focus {
  background-color: rgba(255, 255, 255, 0.9);
  border-radius: 16px 0 16px 0;
}
.weather-wrap {
  margin-top: 30px;
  text-align: center;
}
.location {
  font-size: 2rem;
  font-weight: 600;
  color: #fff;
}
.date {
  color: #ccc;
  font-style: italic;
}
.desc {
  margin-top: 30px;
}
.temp {
  display: inline-block;
  font-size: 72px;
  font-weight: 800;
  line-height: 1;
  color: #fff;

  padding: 28px 24px;
  border-radius: 16px;
  background-color: rgba(255, 255, 255, 0.3);
}
.weather {
  margin-top: 10px;
  font-size: 2rem;
  font-style: italic;
  font-weight: 900;
  color: #fff;
}
</style>
