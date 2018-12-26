<template>
  <div id="app">

    <!-- Top Bar -->
    <top-bar @update="updateDates" v-if="ready"/>

    <!-- Main Content -->
    <div class="content">

      <!-- Left boxes -->
      <div id="box_container">
        <box title="Chuva mm" color="#18afd5" value="0"/>
        <box title="Temp °C" color="#1bf162" :value="boxValues.temp" :min="boxValues.tempMin" :max="boxValues.tempMax"/>
        <box title="Umidade %" color="#fb8b27" :value="boxValues.humid" :min="boxValues.humidMin" :max="boxValues.humidMax"/>
        <box title="Press mmHg" color="#f13c38" value="0" min="--" max="--"/>
        <box title="Vento Km/h" color="#ccc" value="0" min="0" max="--"/>
        <box title="Previsão" color="#18afd5" value="?"/>
      </div>
      
      <!-- <div id="chart_container">
        <graph :data="responseData"/>
      </div> -->
      <div id="table_container">
        <data-table :data="responseData"/>
      </div>

    </div>
    <!-- End main content -->

  </div>
</template>

<script>

const axios = require('axios');
import Chart from 'chart.js';
import TopBar from "./components/TopBar.vue";
import Box from "./components/Box.vue";
import Graph from "./components/Graph.vue";
import DataTable from "./components/DataTable.vue";

export default {
  components: {
    TopBar,
    Box,
    Graph,
    DataTable
  },

  data: function() {
    return {

      dates: {
        start: null,
        end: null,
        filter: 'hours'
      },

      boxValues: {
        temp: null,
        tempMin: null,
        tempMax: null,
        humid: null,
        humidMin: null,
        humidMax: null,
      },           

      responseData: [],
      api: null,
      ready: false,
    }
  },

  mounted() {

    this.api = axios.create({
      //baseURL: 'http://192.168.25.2/api',
      baseURL: 'http://mows.chis.com.br/api',
    });

    // last thing during mount
    this.ready = true;
  },

  computed: {
    
  },

  methods: {

    updateDates(newDates) {
      this.dates.start = newDates.start;
      this.dates.end = newDates.end;
      this.dates.filter = newDates.filter;
      this.refresh();
    },

    refresh () {
      var self = this;
      this.api.get('/now')
      .then((response) => {
        let data = response.data[0];
        self.boxValues.temp = data.temp;
        self.boxValues.tempMin = data.tempMin;
        self.boxValues.tempMax = data.tempMax;
        self.boxValues.humid = data.humid;
        self.boxValues.humidMin = data.humidMin;
        self.boxValues.humidMax = data.humidMax;
      });
      this.getPeriod();
    },

    getPeriod: function () {
      var self = this;
      this.api.get('/period', {
        params: {
          start: self.dates.start,
          end: self.dates.end,
          filter: self.dates.filter
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

#table_container{
	display: block;
	float:left;
	width:	 calc(100% - 88px);
	height: calc(100% - 36px);
	overflow-y: auto;
}
</style>
