<template>
  <div class='box'><div class='box_title'>{{title}}</div>
    <div 
      :class="{
        box_value_small: (min || max),
        box_value: (!min && !max)
      }"
      :style="{color:color}">{{value}}</div>
    <div class='box_value_sub'>{{min}}</div>
    <div class='box_value_sub'>{{max}}</div>
  </div>
</template>

<style>

.box {
  background-color: #222;
  display: block;
  float: left;
  width: 80px;
  height: 68px;
  border-radius: 5px;
  border-color: #666;
  border-style: solid;
  border-width: 1px;
  margin: 3px 3px;
}

.box_title {
  background-color: #000;
  border-radius: 5px;
  color: #fff;
  display: block;
  font-size: 11px;
  padding-top: 0px;
  text-align: center;
  line-height: 18px;
  height: 18px;
}

.box_value {
  color: #ddd;
  display: block;
  font-size: 26px;
  font-weight: normal;
  padding-top: 0px;
  text-align: center;
  line-height: 46px;
  text-shadow: 1px 1px 3px #111;
}
.box_value_small {
  color: #ddd;
  display: block;
  font-size: 26px;
  font-weight: normal;
  padding-top: 0px;
  text-align: center;
  line-height: 34px;
  text-shadow: 1px 1px 3px #111;
}

.box_value_sub {
  color: #ccc;
  float: left;
  display: block;
  font-size: 12px;
  font-weight: normal;
  padding-top: 0px;
  text-align: center;
  width: 40px;
  line-height: 14px;
  text-shadow: 1px 1px  3px #111;
}
</style>

<script>
export default {
  props: {
    title: null,
    value: null,
    min: null,
    max: null,
    color: null
  }
};
</script>
