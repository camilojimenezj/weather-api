<script setup>
defineProps({
  data: {
    type: Object,
    required: true,
  },
});
</script>

<template>
  <div class="container">
    <h1 class="title">Today's Hightlights</h1>
    <div class="box-container">
      <div class="box wind">
        <h2>Wind Status</h2>
        <div class="value">
          {{ data.forecast.forecastday[0].day.maxwind_mph }}<span> mph</span>
        </div>
      </div>
      <div class="box">
        <h2>Humidity</h2>
        <div class="value">
          {{ data.forecast.forecastday[0].day.avghumidity }}<span>%</span>
        </div>
        <div class="percentages">
          <p>0</p>
          <p>50</p>
          <p>100</p>
        </div>
        <div class="bar">
          <div
            class="bar-progress"
            :style="{
              width: data.forecast.forecastday[0].day.avghumidity + '%',
            }"
          ></div>
        </div>
        <div class="percentage-symbol">
          <p>%</p>
        </div>
      </div>
      <div class="box">
        <h2>Visibility</h2>
        <div class="value">
          {{ data.forecast.forecastday[0].day.avgvis_miles }}<span> miles</span>
        </div>
      </div>
      <div class="box">
        <h2>Air Pressure</h2>
        <div class="value">{{ data.current.pressure_mb }}<span> mb</span></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {};
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
  padding: 24px;
  display: flex;
  flex-direction: column;
  align-content: center;
  justify-content: space-between;
  gap: 20px;

  --progress-width: 50%;
}
.box-container {
  display: flex;
  flex-direction: column;
  gap: 20px;
}
.title {
  font-family: "Raleway";
  font-weight: 700;
  font-size: 24px;
  color: #e7e7eb;
}
.box {
  width: 100%;
  padding: 25px 50px;
  background: #1e213a;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;
}
.box.wind {
  height: 100%;
  justify-content: center;
}
.box h2 {
  font-family: "Raleway";
  font-weight: 500;
  font-size: 16px;
  color: #e7e7eb;
}
.box .value {
  font-family: "Raleway";
  font-style: normal;
  font-weight: 700;
  font-size: 64px;
  line-height: 75px;
  color: #e7e7eb;
}
.box .value span {
  font-size: 32px;
  font-weight: 500;
}
.bar {
  width: 100%;
  border-radius: 100vmax;
  height: 8px;
  background: #e7e7eb;
  border: 0;
}
.bar-progress {
  height: 8px;
  border-radius: 100vmax;
  border: 0;
  background: #ffec65;
}
.percentages {
  width: 100%;
  display: flex;
  justify-content: space-between;
  font-family: "Raleway";
  font-weight: 700;
  font-size: 12px;
  color: #a09fb1;
}
.percentage-symbol {
  width: 100%;
  display: flex;
  justify-content: flex-end;
  font-family: "Raleway";
  font-weight: 700;
  font-size: 12px;
  color: #a09fb1;
}
@media screen and (min-width: 1000px) {
  .box-container {
    background-color: #100e1d;
    display: grid;
    grid-template-columns: 1fr 1fr;
    place-items: center;
    gap: 20px;
  }
  .box {
    width: 85%;
  }
}
</style>
