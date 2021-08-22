<template>
  <div
    id="app"
    :class="
      typeof weather.main != 'undefined' && weather.weather[0].main == 'Clouds'
        ? 'cloud'
        : typeof weather.main != 'undefined' &&
          weather.weather[0].main == 'Rain'
        ? 'rain'
        : typeof weather.main != 'undefined' &&
          weather.weather[0].main == 'Snow'
        ? 'snow'
        : typeof weather.main != 'undefined' &&
          weather.weather[0].main == 'Clear'
        ? 'clear'
        : typeof weather.main != 'undefined' && weather.weather[0].main == 'Fog'
        ? 'fog'
        : ''
    "
  >
    <main>
      <div class="title">
        <h2 class="app-title">Weather Info App</h2>
        <p>
          By
          <a href="https://mk-portfolio.vercel.app/" target="_blank"
            >Pythagoras-Dev</a
          >
        </p>
      </div>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Search City...."
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">
            {{ weather.name }},{{ weather.sys.country }}
          </div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}&#176;C</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
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
      //api_key: process.env.VUE_APP_NOT_SECRET_CODE,
      api_key: "4ad35f8133e6a1935619169da5990963",
      // the api base (what goes in front of our request)
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      // where to store the data gotten back
      weather: {},
    };
  },
  methods: {
    fetchWeather: function(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
        )
          // callback
          .then((res) => {
            return res.json();
          })
          .then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results;
    },
    dateBuilder: function() {
      let d = new Date();
      let months = [
        "January",
        "Febuary",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
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
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: "montserrat", sans-serif;
}
#app {
  background-image: url("./assets/images/rain.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.1;
}
#app.cloud {
  background-image: url("./assets/images/cloudy.jpg");
}
#app.clear {
  background-image: url("./assets/images/clear.jpg");
}
#app.rain {
  background-image: url("./assets/images/rain2.jpg");
}
#app.snow {
  background-image: url("./assets/images/snow.jpg");
}
#app.fog {
  background-image: url("./assets/images/fog.jpg");
}
.title {
  background: rgba(238, 238, 238, 0.705);
  padding: 5px 0;
}
.app-title {
  color: #000;
  font-size: 30px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  margin: 5px 0;
}
.title p {
  font-size: 15px;
  text-align: center;
  margin-bottom: 10px;
}
.title a {
  color: black;
}
main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );
}
.search-box {
  width: 70%;
  margin-bottom: 30px;
  margin: 20px auto 30px auto;
}
@media screen and (max-width: 600px) {
  .search-box {
    width: 90%;
    margin-bottom: 30px;
    margin: 0 auto;
  }
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
  border-radius: 0px 16px 0px 16px;
  box-shadow: 0px 0px 8px (0, 0, 0, 0.25);
  transition: 0.4s;
}
.search-box .search-bar:focus {
  background-color: rgba(255, 255, 255, 0.75);
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  border-radius: 16px 0px 16px 0px;
}
.location-box .location {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.location-box .date {
  color: #fff;
  font-size: 20px;
  font-weight: 200;
  text-align: center;
  font-style: italic;
}
.weather-box {
  text-align: center;
}
.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weather-box .weather {
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>
