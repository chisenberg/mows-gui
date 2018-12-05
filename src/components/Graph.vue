<template>
  <canvas id="myChart" :width="width" :height="height"></canvas>
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
      timeLabels: [],
      rainData: [],
      tempData: [],
      humidData: [],
    }
  },

  watch: { 
    data: function(newData) {
      this.updateData(newData);
      this.redraw();
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
    },

    redraw: function () {
      
      var ctx = document.getElementById("myChart").getContext("2d");
        var myChart = new Chart(ctx, {
          type: 'bar',
          data: {
              labels: this.timeLabels,
              datasets: [
                {
                  label: 'Temperature',
                  type: 'line',
                  fill: false,
                  data: this.tempData,
                  backgroundColor: '#1bf162',
                  borderColor: '#1bf162',
                  borderWidth: 2
                },{
                  label: 'Humidity',
                  type: 'line',
                  fill: false,
                  data: this.humidData,
                  backgroundColor: '#fb8b27',
                  borderColor: '#fb8b27',
                  borderWidth: 2
                },{
                  label: 'Rain',
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
              }],
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
