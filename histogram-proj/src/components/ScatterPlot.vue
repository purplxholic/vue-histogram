<!-- translated from http://bl.ocks.org/weiglemc/6185069 -->
<template>
</template>

<script>
import * as d3 from 'd3'
import ChartAxis from './ChartAxis.vue'
export default {
  name: 'ScatterPlot',
  data () {
    return {
      // msg: 'Welcome to Your Vue.js App',
      // random_data: d3.range(1000).map(d3.randomBates(10)),
      width: 960 - this.margin.left - this.margin.right,
      height: 500 - this.margin.top - this.margin.bottom,
      margin: {top: 20, right: 20, bottom: 30, left: 40},
      axisnamey: 'y-axis',
      axisnamex: 'x-axis',
      data: d3.csv('./assets/cereal.csv', function (error, data) {
        data.forEach(function (d) {
          d.Calories = +d.Calories
          d['Protein (g)'] = +d['Protein (g)']
        //    console.log(d);
        })
      }
        )
    }
  },
  computed: {
    xValue () { // data -> value
      let d = this.data
      return d.Calories
    },
    xScale () { // value -> display
      let width = this.width
      return d3.scale.linear().range([0, width])
    },
    xMap () { // data -> display
      let xValue = this.xValue
      let xScale = this.xScale
      let d = this.data
      return xScale(xValue(d))
    },
    // xAxis = d3.svg.axis().scale(xScale).orient("bottom")
    yValue () {  // data -> value
      let d = this.data
      return d['Protein (g)']
    },
    yScale () { // value -> display
      let height = this.height
      return d3.scale.linear().range([height, 0])
    },
    yMap () { // data -> display
      let d = this.data
      let yScale = this.yScale
      let yValue = this.yValue
      return yScale(yValue(d))
    },
    // yAxis = d3.svg.axis().scale(yScale).orient("left")
    formatCount () {
      return d3.format(',.0f')
    },
    contentHeight () {
      let height = this.height
      let margin = this.margin
      return height - margin.top - margin.bottom
    },
    chartTransform () {
      let margin = this.margin
      return 'translate(' + margin.left + ',' + margin.top + ')'
    }
  },
  components: {
    'chart-axis': ChartAxis
  }
}
</script>

<style scoped>
body {
  font: 11px sans-serif;
}

.axis path,
.axis line {
  fill: none;
  stroke: #000;
  shape-rendering: crispEdges;
}

.dot {
  stroke: #000;
}

.tooltip {
  position: absolute;
  width: 200px;
  height: 28px;
  pointer-events: none;
}
</style>
