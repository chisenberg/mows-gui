<template>
  <div id='bar'>

    <div id="btn_box_left">
      <div class="btn btn_selected" id="btn_graph"></div>
      <div class="btn" id="btn_table"></div>
      <div class="btn" id="btn_settings"></div>
      <div class='btn_date'>
        <select id='datepreset' class='date_filter'>
          <option value='hoje'>hoje</option>
          <option value='2dias'>2 dias</option>
          <option value='7dias'>7 dias</option>
          <option value='30dias'>30 dias</option>
          <option value='outro'>outro?</option>
        </select>
      </div>
      <div class='btn_date'>
        <input title='Início' class='date_input' v-model="start" @blur="refresh"/>
        <input title='Fim' class='date_input' v-model="end" @blur="refresh" />
      </div>
      <div class='btn_date'>
        <select id='datefilter' class='date_filter' v-model="filter" @change="refresh">
          <option value='mins'>10min</option>
          <option selected value='hours'>horas</option>
          <option value='days'>dias</option>
          <option value='months'>meses</option>
          <option value='years'>anos</option>
        </select>
      </div>
      <div class="btn" id="btn_update" @click="refresh"></div>
    </div>

    <!-- <div id="btn_box_right">
      <div class="btn" id="btn_min"></div>
      <div class="btn" id="btn_close"></div>
    </div>	 -->

  </div>
</template>


<script>

const moment = require('moment');

export default {
  
  data: function() {
    return { 
        start: '',
        end: '',
        filter: ''
    };
  },

  mounted: function() {
    this.start = moment().subtract(1, 'days').format('DD/MM/YYYY');
    this.end = moment().format('DD/MM/YYYY');
    this.filter = 'hours';
    this.refresh();
  },

  methods: {
    refresh() {
      this.$emit('update', {
        'start': this.start,
        'end': this.end,
        'filter': this.filter
      });
    }
  }

};
</script>

<style>

#bar{
  background-image: url('/images/logo.png');
  background-repeat: no-repeat;
  display: block;
  width: 100%;
  height: 30px;
  cursor:default;

  -webkit-app-region: drag;
}


.btn{
  width: 24px;
  height: 24px;
  display: block;
  background-color: #222;
  background-repeat: no-repeat;
  background-position: center center;
  margin-top:2px;
  margin-left:4px;
  float:left;
  cursor:pointer;
  border-radius: 5px;
  border-style: solid;
  border-color: #666;
  border-width: 1px;

  opacity:0.65;

  -webkit-app-region: no-drag;
}
.btn:hover{
  opacity:1.0;
  background-color: #000;
  border-color: #ddd;
}
.btn_selected, .btn_selected:hover{
  opacity:1.0;
  background-color: #000;
  border-color: #ddd;
  cursor:default;
}

#btn_box_left{
  float: left;
  margin-left: 128px;
}

.btn_date{
  float: left;
  display: block;
  background-color: #222;
  background-repeat: no-repeat;
  background-position: center center;

  margin-top:2px;
  margin-left:4px;
  float:left;

  cursor:pointer;
  border-radius: 5px;
  border-style: solid;
  border-color: #666;
  border-width: 1px;
  -webkit-app-region: no-drag;
}

#btn_box_right{
  float: right;
  margin-right: 2px;
}

#btn_graph{	background-image: url('/images/btn_graph.png'); }
#btn_table{	background-image: url('/images/btn_table.png'); }
#btn_settings{	background-image: url('/images/btn_settings.png'); }
#btn_update{	background-image: url('/images/btn_update.png'); }
#btn_min{	background-image: url('/images/btn_min.png'); }
#btn_close{	background-image: url('/images/btn_close.png'); }

.date_input{
  border-radius: 5px;
  float:left;
  background-color: #222;
  color: #aaa;
  display: block;
  width: 100px;
  height: 24px;
  border: 0;
  text-align: center;
}

.date_input:focus,
.date_filter:focus
{
  outline-color:#eee;
  color: #ddd;
}

.date_input:hover,
.date_filter:hover
{
  background-color: #111;
  color: #fff;
  cursor: pointer;
}

.date_filter{
  border-radius: 5px;
  float: left;
  background-color: #222;
  color: #aaa;
  width: 60px;
  height: 24px;
  display: block;
  font-size: 12px;
  text-align: center;
  line-height: 24px;
  border: 0px;

  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
  text-align-last:center;
  -webkit-appearance: button;
  padding-left: 5px;
  padding-right: 5px;
}
.date_filter option {background-color: #000;}


</style>
