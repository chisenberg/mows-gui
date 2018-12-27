<template>
  <table id="reporttable" class="sortable">
    <thead>
      <tr>
        <th class="sorttable_numeric" style=' width:100px;'>Data</th>
        <th class="sorttable_numeric" style='background-color:#2a6b9d'>Chuva</th>
        
        <th class="sorttable_numeric" style='background-color:#365e34'>T Min</th>
        <th class="sorttable_numeric" style='background-color:#365e34'>T Max</th>
        <th class="sorttable_numeric" style='background-color:#365e34'>T Med</th>
        
        <th class="sorttable_numeric" style='background-color:#825635'>U Min</th>
        <th class="sorttable_numeric" style='background-color:#825635'>U Max</th>
        <th class="sorttable_numeric" style='background-color:#825635'>U Med</th>
        
        <th class="sorttable_numeric" style='background-color:#7c2222'>P Min</th>
        <th class="sorttable_numeric" style='background-color:#7c2222'>P Max</th>
        <th class="sorttable_numeric" style='background-color:#7c2222'>P Med</th>
        
        <th class="sorttable_numeric" style='background-color:#575757'>V Max</th>
        <th class="sorttable_numeric" style='background-color:#575757'>V Med</th>
      </tr>
    </thead>
    
    <tbody>
      <tr v-for="row in arrayData" :key="row.formattedtime">
        <td>{{row.formattedtime}}</td>
        <td>{{row.rain || '---'}}</td>
        <td>{{row.temp_min || '---'}}</td>
        <td>{{row.temp_max || '---'}}</td>
        <td>{{row.temperature || '---'}}</td>
        <td>{{row.humid_min || '---'}}</td>
        <td>{{row.humid_max || '---'}}</td>
        <td>{{row.humidity || '---'}}</td>
        <td>{{row.press_min || '---'}}</td>
        <td>{{row.press_max || '---'}}</td>
        <td>{{row.pressure || '---'}}</td>
        <td>{{row.wind_speed_max || '---'}}</td>
        <td>{{row.wind_speed || '---'}}</td>
      </tr>
    </tbody>

  </table>
</template>


<script>

export default {

  props: {
    data: {
      type: Array,
      default: []
    }
  },

  watch: { 
    data: function(newData) {
      this.arrayData = newData;
    }
  },

  data: function() {

    return { 
      arrayData: []
    };

  },

  mounted: function() {
    this.arrayData = this.data;
  },
  
  methods: {
    refresh() {
     
    }
  }

};
</script>



<style>

#table_container::-webkit-scrollbar {
  width: 14px;
}

#table_container::-webkit-scrollbar-track {
  -webkit-box-shadow: inset 0 0 0px rgba(0,0,0,0.5); 
  -webkit-border-radius: 0px;
  border-radius: 0px;
}

#table_container::-webkit-scrollbar-thumb {
  -webkit-border-radius: 0px;
  border-radius: 5px;
  background-color: #666;

  border-radius: 5px;
	border-style: solid;
	border-color: #999;
	border-width: 1px;

   /*-webkit-box-shadow: inset 0 0 6px rgba(100,100,100,0.8); */
}

#reporttable{
	float:left;	
	width:100%;
	table-layout: fixed;
	
	border-collapse: collapse;
	font-family: 'Open Sans';
	font-size: 12px;
	text-align: center;

	color: #eee;
	text-shadow: 0px 0px 3px black;

	margin-top: 2px;
}


.sortable tr:nth-child(even) {
    background-color: #2f2f2f;
}

/* Sortable tables */
.sortable table thead th  {
	position:absolute;   
   	cursor: pointer;
	color: #aaa;
}

.sortable td, .sortable th { border: solid 1px #666;}
.sortable th{font-weight: normal;}
.sortable tr:hover  {color: #fff; background-color: #444;}


</style>
