<template>
  <div id="app">

    <!-- Top Bar -->
    <top-bar @refresh="refresh" :dates="dates" @update="updateDates"/>

    <!-- Main Content -->
    <div class="content">

      <!-- Left boxes -->
      <div id="box_container">
        <box title="Chuva mm" color="#18afd5" value="0"/>
        <box title="Temp °C" color="#1bf162" :value="temp" :min="tempMin" :max="tempMax"/>
        <box title="Umidade %" color="#fb8b27" :value="humid" :min="humidMin" :max="humidMax"/>
        <box title="Press mmHg" color="#f13c38" value="0" min="--" max="--"/>
        <box title="Vento Km/h" color="#ccc" value="0" min="0" max="--"/>
        <box title="Previsão" color="#18afd5" value="?"/>
      </div>
      
      <div id="chart_container">
        <graph :data="responseData"/>
      </div>

    </div>
    <!-- End main content -->

  </div>
</template>

<script>

const axios = require('axios');
const moment = require('moment');
import Chart from 'chart.js';

import TopBar from "./components/TopBar.vue";
import Box from "./components/Box.vue";
import Graph from "./components/Graph.vue";

export default {
  components: {
    TopBar,
    Box,
    Graph
  },

  data: function() {
    return {

      dates: {
        start: null,
        end: null
      },

      temp: null,
      tempMin: null,
      tempMax: null,
      humid: null,
      humidMin: null,
      humidMax: null,

      responseData: [],

      lastUpdate: null,

      api: null,

    }
  },

  mounted() {

    this.api = axios.create({
      baseURL: 'http://192.168.25.2/api',
    });

    this.dates.start = moment().subtract(1, 'days').format('DD/MM/YYYY');
    this.dates.end = moment().format('DD/MM/YYYY');

    this.refresh();
    
    // setInterval(()=>{
    //   this.refresh();
    // },10000);

  },

  computed: {
    lastUpdatePretty () {
      if(this.lastUpdate) {
        return this.lastUpdate.format("DD/MM/YY HH:mm");
      }
    }
  },

  methods: {

    updateDates(newDates) {
      console.log(newDates);
      this.dates.start = newDates.start;
      this.dates.end = newDates.end;
      this.refresh();
    },

    refresh () {
      var self = this;
      this.api.get('/now')
      .then((response) => {
        let data = response.data[0];
        self.temp = data.temp;
        self.tempMin = data.tempMin;
        self.tempMax = data.tempMax;
        self.humid = data.humid;
        self.humidMin = data.humidMin;
        self.humidMax = data.humidMax;
        self.lastUpdate = moment(data.time, "YYYYMMDDHHmm");
      });

      this.getPeriod();
    },

    getPeriod: function () {
      var self = this;
      this.api.get('/period', {
        params: {
          start: self.dates.start,
          end: self.dates.end,
          filter: 'hours'
        }
      })
      .then((response) => {
        this.responseData = response.data;
      });
    }
    
  }


};
</script>

<style>

body {
  margin: 0px;
  border-radius: 0px;
  font-family: "Open Sans";
  color:azure;
  background-color: rgba(0, 0, 0, 0);
  background-image: url("/images/back.png");
}

#content{
  float:left;
  display:block;
  width: 100%;
}


#box_container{
  display: block;
  float:left;
  width:	88px;
}

#chart_container{
  display: block;
  float:left;
  width:	 calc(100% - 98px);
  height: 100%;
}
</style>
