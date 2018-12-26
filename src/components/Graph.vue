<template>
  <canvas id="chart" :width="width" :height="height"></canvas>
</template>

<style>

</style>

<script>

export default {

  props: {
    width: 0,
    height: 0,
    data: {
      type: Array,
      default: []
    }
  },

  data: function () {
    return {
      chart: null,

      timeLabels: [],
      rainData: [],
      tempData: [],
      humidData: [],
    }
  },

  watch: { 
    data: function(newData) {
      this.updateData(newData);
    }
  },

  methods: {

    updateData: function (newData) {
      this.timeLabels = [];
      this.rainData = [];
      this.tempData = [];
      this.humidData = [];
      newData.forEach((d, idx) => {
        this.timeLabels.push(d.formattedtime);
        this.rainData.push(d.rain);
        this.tempData.push(d.temperature);
        this.humidData.push(d.humidity);
      });
      this.redraw();
    },

    redraw: function () {
      if(this.chart) {
        this.chart.destroy();
      }
      var ctx = document.getElementById("chart").getContext("2d");
      this.chart = new Chart(ctx, {
        type: 'bar',
        data: {
          labels: this.timeLabels,
          datasets: [
            {
              label: 'Temperatura',
              type: 'line',
              fill: false,
              data: this.tempData,
              backgroundColor: '#1bf162',
              borderColor: '#1bf162',
              borderWidth: 2
            },{
              label: 'Umidade',
              type: 'line',
              fill: false,
              data: this.humidData,
              backgroundColor: '#fb8b27',
              borderColor: '#fb8b27',
              borderWidth: 2
            },{
              label: 'Pressão',
              data: this.rainData,
              backgroundColor: '#f13c38',
              borderColor: '#f13c38',
              borderWidth: 1
            },{
              label: 'Vento',
              data: this.rainData,
              backgroundColor: '#ccc',
              borderColor: '#ccc',
              borderWidth: 1
            },{
              label: 'Chuva',
              data: this.rainData,
              backgroundColor: '#18afd5',
              borderColor: '#18afd5',
              borderWidth: 1
            },
          ]
        },
        options: {
          scales: {
            xAxes: [{
              maxBarThickness: 20,
            }]
          },
          legend: {
            display: true,
            position: 'top',
            labels: {
                fontColor: '#ccc'
            }
          }
        }
      });
    }
  },


  mounted () {
    if(this.data) {
      this.updateData(this.data);
    }
    this.redraw();
  }
};
</script>
