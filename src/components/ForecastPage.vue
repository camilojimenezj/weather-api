<script setup>
defineProps({
  data: {
    type: Object,
    required: true,
  },
  images: {
    type: Array,
  },
  metric: {
    type: String,
  },
});
</script>

<template>
  <div class="container">
    <div v-for="(el, i) in data.forecast.forecastday" :key="el.id" class="box">
      <h3 class="date">{{ getDate[i] }}</h3>
      <img :src="images[i]" alt="img" />
      <div class="temperatures">
        <p>
          {{ Math.round(el.day["mintemp_" + metric]) }}°{{
            metric.toUpperCase()
          }}
        </p>
        <p class="max">
          {{ Math.round(el.day["maxtemp_" + metric]) }}°{{
            metric.toUpperCase()
          }}
        </p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      dates: ["Today", "Tomorrow"],
    };
  },
  computed: {
    getDate() {
      const date = new Date(
        this.data.forecast.forecastday[2].date_epoch * 1000
      );
      let dateString = date.toUTCString();

      return this.dates.concat(dateString.slice(0, 12));
    },
  },
};
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.container {
  width: 100%;
  background-color: #100e1d;
  padding: 50px;
  display: grid;
  grid-template-columns: 1fr 1fr;
  place-items: center;
  gap: 20px;
}
.box {
  width: 120px;
  height: 177px;
  padding: 17px;
  background: #1e213a;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  place-items: center;
}
.date {
  text-align: center;
  width: 100%;
  font-family: "Raleway";
  font-weight: 500;
  font-size: 16px;
  line-height: 19px;
  color: #e7e7eb;
}
img {
  width: 56.44px;
}
.temperatures {
  width: 100%;
  display: flex;
  justify-content: space-between;
  font-family: "Raleway";
  font-weight: 500;
  font-size: 16px;
  color: #e7e7eb;
}
.temperatures .max {
  color: #a09fb1;
}
@media screen and (min-width: 1000px) {
  .container {
    padding-bottom: 0;
    justify-content: center;
  }
}
</style>
