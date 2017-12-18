<template>


  <svg :width="width" :height="height" >
          <g v-for="bin in bins" :transform="'translate('+margin.left+','+margin.top+')'">
            <rect
              :x="1"
              :transform="'translate(' + x(bin.x0) + ',' + y(bin.length) + ')'"
              :width="x(bin.x1) - x(bin.x0)-1 "
              :height="height - y(bin.length)">
            </rect>
            <!-- <text

              dy="0.75em"
              text-anchor="middle"
              :y="6"
              :x="(x(bin.x1) - x(bin.x0)) / 2"
              :text="formatCount(bin.length)"
              >
            </text> -->

          </g>
          <g
            :transform="'translate(0,'+height+')'"
            :call="xaxis(x)"
            >


          </g>
  </svg>

</template>
<!-- :dy=".75em" -->
<!-- :text-anchor="middle" -->
<script>
import * as d3 from 'd3'
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
  methods: {
    xaxis: function (event) {
      let x = this.x
      return d3.axisBottom(x)
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
    labels () {
      let bins = this.bins
      let formatCount = d3.format(',.0f')
      return formatCount(bins.length)
    },
    yaxis () {
      let y = this.y
      return d3.axisLeft(y)
    },
    formatCount () {
      return d3.format(',.0f')
    }
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
</style>
