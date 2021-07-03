<template>
  <div id="app" class="container my-5">
    <h1>Covid-19 in USA dashboard</h1>
    <div class="row mt-5">
      <div class="col">
        <h2>Positive</h2>
        <line-chart :chartData="arrPositive" :options="chartOptions" label="Positive" />
      </div>
    </div>
    <div class="row mt-5">
      <div class="col">
        <h2>Hospitalized</h2>
        <line-chart :chartData="arrHospitalized" :options="chartOptions" label="Hospitalized"></line-chart>
      </div>
    </div>
    <div class="row mt-5">
      <div class="col">
        <h2>In Icu</h2>
        <line-chart :chartData="arrInIcu" :options="chartOptions" label="inicu"></line-chart>
      </div>
    </div>
    <div class="row mt-5">
      <div class="col">
        <h2>On Ventilators</h2>
        <line-chart :chartData="arrOnVentilators" :options="chartOptions" label="onventilators"></line-chart>
      </div>
    </div>
    <div class="row mt-5">
      <div class="col">
        <h2>Recovered</h2>
        <line-chart :chartData="arrRecovered" :options="chartOptions" label="recovered"></line-chart>
      </div>
    </div>
    <div class="row mt-5">
      <div class="col">
        <h2>Deaths</h2>
        <line-chart :chartData="arrDeaths" :options="chartOptions" label="deaths"></line-chart>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import moment from 'moment'

import LineChart from './components/LineChart.vue'

export default {
  name: 'App',
  components: {
    'line-chart': LineChart
  },
  data() {
    return {
      arrPositive:[],
      arrHospitalized:[],
      arrInIcu:[],
      arrOnVentilators:[],
      arrRecovered:[],
      arrDeaths:[],
      chartOptions:{
        responsive:true,
        maintainAspectRatio: false
      }
    }
  },
  async created(){
    const {data} = await axios.get("https://covidtracking.com/api/us/daily")

    data.forEach(d => {
      const date = moment(d.date, 'YYYY/MM/DD').format('MM/DD/YYYY');

      const {
        positive,
        hospitalizedCurrently,
        inIcuCurrently,
        onVentilatorCurrently,
        recovered,
        death
      } = d;

      this.arrPositive.push({date, total: positive})
      this.arrHospitalized.push({date, total: hospitalizedCurrently})
      this.arrInIcu.push({date, total: inIcuCurrently})
      this.arrOnVentilators.push({date, total: onVentilatorCurrently})
      this.arrRecovered.push({date, total: recovered})
      this.arrDeaths.push({date, total: death})
    });
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
