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
      pressData: [],
      windData: [],
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
      this.pressData = [];
      this.windData = [];
      newData.forEach((d, idx) => {
        this.timeLabels.push(d.formattedtime);
        this.rainData.push(d.rain || d.temperature / 30);
        this.tempData.push(d.temperature);
        this.humidData.push(d.humidity);
        this.windData.push(d.wind || d.humidity / 20);
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
              yAxisID: 0,
              fill: false,
              data: this.tempData,
              backgroundColor: '#1bf162',
              borderColor: '#1bf162',
              borderWidth: 2
            },{
              label: 'Umidade',
              type: 'line',
              yAxisID: 1,
              fill: false,
              data: this.humidData,
              backgroundColor: '#fb8b27',
              borderColor: '#fb8b27',
              borderWidth: 2,
              borderDash: [10,4]
            },{
              label: 'Pressão',
              yAxisID: 2,
              data: this.pressData,
              hidden: true,
              backgroundColor: '#f13c38',
              borderColor: '#f13c38',
              borderWidth: 1
            },
            {
              label: 'Vento',
              type: 'line',
              fill: false,
              yAxisID: 3,
              data: this.windData,
              hidden: true,
              borderColor: '#fff',
              borderWidth: 1,
              borderDash: [5,2]
            },
            {
              label: 'Chuva',
              yAxisID: 4,
              data: this.rainData,
              hidden: true,
              backgroundColor: '#222',
              borderColor: '#18afd5',
              borderWidth: 2
            },
          ]
        },
        options: {
          scales: {
            xAxes: [{
              maxBarThickness: 30,
              ticks: {
                fontColor: '#ccc',
              },
            }],
            yAxes: [
              {
                id: 0,
                ticks: {
                  fontColor: '#1bf162',
                  suggestedMax: 35,
                  suggestedMin: 16
                },
              },
              {
                id: 1,
                ticks: {
                  fontColor: '#fb8b27',
                  suggestedMin: 0,
                  suggestedMax: 100
                },
              },
              {
                id: 2,
                position: 'right',
                ticks: {
                  fontColor: '#f13c38',
                  suggestedMin: 0
                },
              },
              {
                id: 3,
                position: 'right',
                ticks: {
                  fontColor: '#ccc',
                  suggestedMin: 0
                },
              },
              {
                id: 4,
                ticks: {
                  fontColor: '#18afd5',
                  suggestedMin: 0,
                  suggestedMax: 10
                },
              }
            ]
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
