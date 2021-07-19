<template>
  <div id="app" class="container">
     <div class="row mt-5" v-if="arrPositive.length > 0">
       <div class="col">
          <h2>positive</h2>
          <line-chart :label='label' :chartData="arrPositive" :options="chartOptions"></line-chart>
       </div>
     </div>
  </div>
</template>

<script>

import  axios from 'axios';
import moment from 'moment';
import LineChart from './components/LineChart.vue';
export default {
  name: 'App',
  components: {
    LineChart
  },
  data() {
    return {
      label: "Positive",
      chartOptions: {
        responsive: true,
        maintainAspectRatio: false
      },
        arrHospitalized:[],
        arrPositive: [],
        arrInIcu: [],
        arrOnVentilator:[],
        arrRecoverd:[],
        arrDeaths:[],

    }
  },
  async created() {
         // https://covidtracking.com/api/us/daily
         const {data}=await axios.get("http://localhost:8000/api/us/daily");
         data.forEach(d => {
           const date= moment(d.date,"YYYYMMDD").format("MMDD");
           const {
             positive,
             hospitalizedCurrently,
             inIcuCurrently,
             onVentilatorCurrently,
             recovered,
             death
           } = d;
           this.arrPositive.push({date, total:positive});
           this.arrHospitalized.push({date, total:hospitalizedCurrently});
           this.arrInIcu.push({date, total:inIcuCurrently});
           this.arrOnVentilator.push({date, total:onVentilatorCurrently});
           this.arrRecoverd.push({date, total:recovered});
           this.arrDeaths.push({date, total:death});
         });
    //  await axios.get("http://localhost:8000/events")
    //    .then(response => (console.log(response.data)))
    //  console.log(events);
  },
}
</script>

<style>

</style>
