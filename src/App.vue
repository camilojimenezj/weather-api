<script setup>
import MainPage from "./components/MainPage.vue";
import ForecastPage from "./components/ForecastPage.vue";
import Hightlights from "./components/Hightlights.vue";
</script>

<template>
  <div class="container" v-if="info">
    <div class="metric">
      <input type="radio" v-model="metrics" id="c" value="c" />
      <label for="c">°C</label>
      <input type="radio" v-model="metrics" id="f" value="f" />
      <label for="f">°F</label>
    </div>
    <MainPage
      :data="info"
      :img="weatherImg"
      :metric="metrics"
      @fetch="(i) => apiRequest(i)"
      class="main-page"
    />
    <div class="forecast-container">
      <ForecastPage
        :data="info"
        :images="forecastImg"
        :metric="metrics"
        class="forecast"
      />
      <Hightlights :data="info" class="hightlights" />
    </div>
  </div>
  <div class="loader" v-if="loading">
    <img src="/images/rings.svg" alt="loader" />
  </div>
</template>

<script>
export default {
  name: "Parent",
  components: { MainPage },
  data() {
    return {
      info: null,
      loading: false,
      metrics: "c",
      weatherImg: "/images/Clear.png",
      forecastImg: [],
      conditions: {
        "/images/Clear.png": ["Clear", "Sunny"],
        "/images/LightCloud.png": [
          "Partly Sunny",
          "Mostly Sunny",
          "Partly cloudy",
        ],
        "/images/HeavyCloud.png": ["Overcast"],
        "/images/Shower.png": ["Showers", "Scattered Showers"],
        "/images/Sleet.png": ["Rain and Snow", "Snow Showers", "Sleet"],
        "/images/Snow.png": ["Light Snow", "Snow"],
        "/images/HeavyRain.png": ["Heavy rain"],
        "/images/LightRain.png": [
          "Patchy rain possible",
          "Light rain",
          "Moderate rain",
        ],
        "/images/Thunderstorm.png": ["Scattered Thunderstorms", "Thunderstorm"],
      },
    };
  },

  methods: {
    imgUrl() {
      for (let url in this.conditions) {
        if (this.conditions[url].includes(this.info.current.condition.text)) {
          this.weatherImg = url;
        }
        if (
          this.conditions[url].includes(
            this.info.forecast.forecastday[0].day.condition.text
          )
        ) {
          this.forecastImg[0] = url;
        }
        if (
          this.conditions[url].includes(
            this.info.forecast.forecastday[1].day.condition.text
          )
        ) {
          this.forecastImg[1] = url;
        }
        if (
          this.conditions[url].includes(
            this.info.forecast.forecastday[2].day.condition.text
          )
        ) {
          this.forecastImg[2] = url;
        }
      }
    },
    apiRequest(city = "medellin") {
      this.loading = true;
      fetch(
        `https://api.weatherapi.com/v1/forecast.json?key=72e59c6f35184e1c99f223248221707&q=${city}&days=3&aqi=no&alerts=no`
      )
        .then((response) => response.json())
        .then((json) => {
          this.info = json;
          this.imgUrl();
          this.loading = false;
        });
    },
  },
  beforeMount() {
    this.apiRequest();
  },
};
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.metric {
  display: none;
}

/* loader */
.loader {
  position: fixed;
  width: 100%;
  height: 100vh;
  top: 0;
  left: 0;
  display: grid;
  place-content: center;
  background: #100e1d4d;
}
/* Metric toggle */
.metric {
  display: flex;
  position: absolute;
  top: 10px;
  right: 60px;
}
.metric input[type="radio"] {
  display: none;
}

.metric label {
  font-family: "Raleway";
  font-weight: 600;
  margin-left: 5px;
  width: 40px;
  height: 40px;
  background: #585676;

  color: #e7e7eb;
  border-radius: 50%;
  cursor: pointer;
  display: grid;
  place-content: center;
}

.metric input[type="radio"]:checked + label {
  background: #e7e7eb;
  color: #585676;
}
@media screen and (min-width: 1000px) {
  .container {
    display: flex;
  }
  .main-page {
    width: 25%;
  }
  .forecast-container {
    width: 75%;
    height: 100vh;
    background-color: #100e1d;
  }
}
</style>
