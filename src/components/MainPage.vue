<script setup>
defineProps({
  data: {
    type: Object,
    required: true,
  },
  img: {
    type: String,
    required: true,
  },
  metric: {
    type: String,
  },
});
</script>

<template>
  <div class="container">
    <nav>
      <button class="search-for" v-on:click="modalActive">
        Search for places
      </button>
      <button class="location-btn">
        <img
          src="../assets/my_location_black_24dp.svg"
          alt="location"
          @click="location"
        />
      </button>
    </nav>
    <img src="../assets/Cloud-background.png" alt="clouds" class="cloud-bg" />
    <img :src="img" alt="img" class="weather-img" />
    <h1 class="number">
      {{ Math.round(data.current["temp_" + metric])
      }}<span>°{{ metric.toUpperCase() }}</span>
    </h1>
    <h2 class="status">{{ data.current.condition.text }}</h2>
    <h3 class="date">
      Today <span>•</span>
      {{ getToday }}
    </h3>
    <h3 class="location">
      {{ data.location.name + ", " + data.location.region }}
    </h3>
    <div class="modal-search">
      <button class="close" @click="modalActive">X</button>
      <div class="search-container">
        <input
          type="text"
          name="query"
          id="query"
          class="search-bar"
          placeholder="search location"
          v-model="city"
          @keypress="geolocationFetch(city)"
        />
        <button
          @click="
            $emit('fetch', city);
            modalActive();
          "
        >
          Search
        </button>
      </div>
      <div class="results-container" v-if="geo">
        <button
          class="result"
          v-for="el in geo.results"
          :key="el.id"
          @click="
            $emit('fetch', `${el.name} ${el.admin1} ${el.country}`);
            modalActive();
          "
        >
          {{ el.name + ", " + el.admin1 + ", " + el.country }}
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      city: "medellin",
      geo: null,
    };
  },
  methods: {
    modalActive() {
      const $modal = document.querySelector(".modal-search");
      $modal.classList.toggle("active");
    },
    geolocationFetch(place) {
      fetch("https://geocoding-api.open-meteo.com/v1/search?name=" + place)
        .then((res) => res.json())
        .then((json) => (this.geo = json));
    },
    location() {
      const options = {
        enableHighAccuracy: true,
        timeout: 5000,
        maximumAge: 0,
      };
      const success = (pos) => {
        const crd = pos.coords;
        const fetchLocation = async () => {
          const res = await fetch(
            `https://api.bigdatacloud.net/data/reverse-geocode-client?latitude=${crd.latitude}&longitude=${crd.longitude}&localityLanguage=en`
          );
          const json = await res.json();
          console.log(this.city, crd.latitude, crd.longitude, json);
          this.$emit("fetch", `${json.locality} ${json.principalSubdivision}`);
        };
        console.log(crd);
        fetchLocation();
      };
      function error(err) {
        console.warn(`ERROR(${err.code}): ${err.message}`);
      }
      navigator.geolocation.getCurrentPosition(success, error, options);
    },
  },
  computed: {
    getToday() {
      let date = new Date(this.data.forecast.forecastday[0].date_epoch * 1000);
      let dateString = date.toUTCString();
      return dateString.slice(0, 12);
    },
  },
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Raleway:wght@500&display=swap");

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Raleway", monospace;
  color: #a09fb1;
}
.container {
  background: #1e213a;
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  min-height: 100vh;
  height: inherit;
}
nav {
  width: 100%;
  padding: 10px;
  display: flex;
  justify-content: space-between;
}
nav .search-for {
  width: 161px;
  height: 40px;
  color: #fff;
  background: #6e707a;
  box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
  border: none;
  cursor: pointer;
}
nav .location-btn {
  color: #fff;
  background: #6e707a;
  border-radius: 50%;
  display: grid;
  place-content: center;
  cursor: pointer;
  box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
  border: none;
  width: 40px;
  height: 40px;
}
.cloud-bg {
  position: absolute;
  width: inherit;
  opacity: 0.1;
  margin-top: 100px;
}
.weather-img {
  margin-top: 10px;
}
.number {
  margin-top: 20px;
  font-style: normal;
  font-weight: 500;
  font-size: 144px;
  line-height: 169px;
  color: #e7e7eb;
}
.number span {
  font-weight: 600;
  font-size: 36px;
  line-height: 42px;
  text-align: center;
}
.status {
  text-align: center;
  font-weight: 600;
  font-size: 36px;
  margin-top: 10px;
}
.date {
  margin-top: 48px;
  font-weight: 500;
  font-size: 18px;
  line-height: 21px;
  color: #88869d;
}
.date span {
  margin: 0 12px;
}
.location {
  font-weight: 600;
  font-size: 18px;
  color: #88869d;
  margin-top: 33px;
}
/* Modal */
.modal-search {
  position: absolute;
  background: #1e213a;
  width: inherit;
  height: 100vh;
  transform: translate(-100%, 0);
  transition: all 0.3s ease-out;
}
.search-container {
  margin-top: 80px;
  padding: 20px;
  display: flex;
  justify-content: space-between;
}
.search-container .search-bar {
  width: 65%;
  height: 48px;
  left: 13px;
  top: 60px;
  border: 1px solid #e7e7eb;
  background: transparent;
  padding: 20px;
}
.search-container button {
  width: 30%;
  height: 48px;
  left: 277px;
  top: 60px;
  color: #e7e7eb;
  background: #3c47e9;
  border: 0;
  box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
  cursor: pointer;
}
.search-container button:hover {
  opacity: 0.8;
}
.search-container button:active {
  background: rgb(144, 175, 216);
  color: #3c47e9;
  border-radius: 5px;
}
.close {
  position: absolute;
  top: 10px;
  right: 15px;
  cursor: pointer;
  background: none;
  border: none;
}
.active {
  transform: translate(0, 0);
  transition: all 0.3s ease-in;
}
/* Results */
.results-container {
  display: flex;
  flex-direction: column;
  padding: 10px 20px;
}
.result {
  height: 40px;
  background: none;
  border: none;
  cursor: pointer;
}
.result:hover {
  border: 1px solid #3c47e9;
}
</style>
