<template>
  <div
    id="app"
    :class="typeof weather.main != 'undefined' && 
  weather.main.temp > 16 ? 'warm' : ''"
  >
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

      <div class="row" v-if="typeof weather.main != 'undefined'">
        <div class="column">
          <div class="location-box">
            <div class="location">{{weather.name}}, {{weather.sys.country}}</div>
            <div class="date">{{dataBuilder()}}</div>
            <div class="temp">{{Math.round(weather.main.temp)}}°c</div>
          </div>
        </div>
        <div class="column">
          <div class="weather-box">
            <div class="weather">{{weather.weather[0].main}}</div>
            <div class="humidity">Humedad: {{weather.main.humidity}}%</div>
            <div class="minima">Minima del día: {{Math.round(weather.main.temp_min)}}°c</div>
            <div class="maxima">Máxima del día: {{Math.round(weather.main.temp_max)}}°c</div>
            <div class="speed">Velocidad del viento: {{weather.wind.speed}} km/h</div>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      api_key: "8d4413b8e5d8f7668063f2aaebfb37b7",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {}
    };
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.url_base}weather?q=${this.query}&units=metric&appid=${this.api_key}`
        )
          .then(res => {
            const data = res.json();
            console.log(data);
            return data;
          })
          .then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results;
    },
    dataBuilder() {
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

      return `${day} ${date} ${month} ${year}`;
    }
  }
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
  background-image: url("./assets/cold-bg.jpg");
  background-size: cover;
  background-position: botton;
  transition: 0.4s;
}

#app.warm {
  background-image: url("./assets/warm-bg.jpg");
}

main {
  min-height: 100vh;
  padding: 25px;

  background-image: linear-gradient(
    to botton,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );
}

.row {
  display: flex;
  flex-flow: column;
  flex-direction: row;
  flex-wrap: wrap;
  width: 100%;
}

.column {
  display: flex;
  flex-direction: column;
  flex-basis: 100%;
  flex: 1;
}

.search-box {
  width: 100%;
  margin-bottom: 30px;
}

.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;
  color: #313131;
  font-size: 20px;
  appearance: none;
  border: solid;
  outline: none;
  background: none;
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 10px 0px 16px;
  transition: 0.5s;
}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 12px, rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 16px 16px 16px;
}

.location-box {
  text-align: center;
  text-shadow: 1px 2px rgba(0, 0, 0, 0.25);
}

.location-box .location {
  color: rgb(27, 34, 4);
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 2px rgba(0, 0, 0, 0.25);
}

.location-box .date {
  color: rgb(20, 14, 14);
  font-size: 16px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}

.location-box .temp {
  display: inline-block;
  padding: 10px 15px 15px 10px;
  color: rgb(29, 27, 27);
  font-size: 100px;
  font-weight: auto;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 16px;
  margin: 10px 10px;

  box-shadow: 3px 6px rgba(0, 0, 0, 0.5);
}

.weather-box {
  text-align: center;
  text-shadow: 1px 2px rgba(0, 0, 0, 0.25);
}

.weather-box .weather {
  color: rgb(5, 8, 12);
  font-size: 24px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3p 6px rgba(0, 0, 0, 0.25);
}

.weather-box .humidity {
  color: rgb(5, 8, 12);
  font-size: 24px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3p 6px rgba(0, 0, 0, 0.25);
}

.weather-box .minima {
  color: rgb(5, 8, 12);
  font-size: 24px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3p 6px rgba(0, 0, 0, 0.25);
}

.weather-box .maxima {
  color: rgb(5, 8, 12);
  font-size: 24px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3p 6px rgba(0, 0, 0, 0.25);
}

.weather-box .speed {
  color: rgb(5, 8, 12);
  font-size: 24px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3p 6px rgba(0, 0, 0, 0.25);
}

@media only screen and (max-width: 767px) {
  .column {
    width: auto !important;
    float: none;
    margin-left: 0;
    margin-right: 0;
    padding: 10px 30px;
  }
}

@media only screen and (max-width: 460px) {
  .row {
    width: auto;
  }
}

/* mobile wide/smaller tablets
--------------------------------------------------------------- */

@media screen and (min-width: 1200px) {
  .wide .row {
    max-width: 1180px;
  }
}
</style>
