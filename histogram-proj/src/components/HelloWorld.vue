<template>


  <svg :width="width" :height="height" >
          <g v-for="bin in bins" :transform="'translate('+margin.left+','+margin.top+')'">
            <rect
              :x="1"
              :transform="'translate(' + x(bin.x0) + ',' + y(bin.length) + ')'"
              :width="x(bin.x1) - x(bin.x0)-1"
              :height="height - y(bin.length)">
            </rect>
            <text
              dy='0.75em'
              text-anchor="middle"
              :y='6'
              :x="(x(bin.x1) - x(bin.x0)) / 2"
              :transform="'translate(' + x(bin.x0) + ',' + y(bin.length) + ')'">
              {{ formatCount(bin.length) }}
            </text>

          </g>
          <!-- <g>
            <path class="domain" stroke="#000" d="M0.5,6V0.5H880.5V6"></path>
            <g class="tick" opacity="1" transform="'translate(0.5,0)'">
              <line stroke="#000" y2="6"></line>
              <text fill="#000" y="9" dy="0.71em">0.0</text>
            </g>
          </g> -->
          <g :transform="chartTransform">
          <g><chart-axis orient="Left" :scale=y /></g>
          <g :transform="'translate(0,' + contentHeight +')'"><chart-axis orient="Bottom" :scale=x /></g>
        </g>
  </svg>

</template>

<script>
import * as d3 from 'd3'
import ChartAxis from './ChartAxis.vue'
export default {
  name: 'HelloWorld',
  data () {
    return {
      // msg: 'Welcome to Your Vue.js App',
      random_data: d3.range(1000).map(d3.randomBates(10)),
      width: 960,
      height: 500,
      margin: {top: 10, right: 30, bottom: 30, left: 30}
    }
  },
  computed: {
    bins () {
      let x = this.x
      let data = this.random_data
      return d3.histogram().domain(x.domain()).thresholds(x.ticks(20))(data)
    },
    x () {
      let width = this.width
      return d3.scaleLinear().rangeRound([0, width])
    },
    y () {
      let height = this.height
      let bins = this.bins
      return d3.scaleLinear().domain([0, d3.max(bins, function (d) { return d.length })]).range([height, 0])
    },
    yaxis () {
      let y = this.y
      return d3.axisLeft(y)
    },
    formatCount () {
      return d3.format(',.0f')
    },
    xaxis () {
      let x = this.x
      console.log(d3.axisBottom(x))
      return d3.axisBottom(x)
    },
    contentHeight () {
      let height = this.height
      let margin = this.margin
      return height - margin.top - margin.bottom
    },
    chartTransform () {
      let margin = this.margin
      return 'translate('+margin.left+','+margin.top+')'
    }
  },
  components: {
    'chart-axis': ChartAxis
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
rect {
  fill: steelblue;
}

text {
  fill: #fff;
  font: 10px sans-serif;
}

.chart-axis.text{
  fill: #000;
}
</style>
