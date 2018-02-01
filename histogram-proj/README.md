# histogram-proj

> A Vue.js project

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report

# run unit tests
npm run unit

# run e2e tests
npm run e2e

# run all tests
npm test
```

For a detailed explanation on how things work, check out the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).

Explanation

```javascript
<template>


  <svg :width="truew" :height="trueh" >
          <g v-for="bin in bins" :transform="'translate('+margin.left+','+margin.top+')'">
            <rect
              :x="1"
              :transform="'translate(' + x(bin.x0) + ',' + y(bin.length) +')'" //to move the bars
              :width="x(bin.x1) - x(bin.x0)-1" // -1 is to create a gap between the bars
              :height="height - y(bin.length)- 30 "> // controls the height of the rectangles drawn & range of data drawn out
            </rect>
            <text
              dy='0.75em'
              text-anchor="middle"
              :y='6'
              :x="(x(bin.x1) - x(bin.x0)) / 2"
              :transform="'translate(' + x(bin.x0) + ',' + y(bin.length) + ')'">
              {{ formatCount(bin.length) }} //write out the label
            </text>

          </g>
          <g class="chart-axis" :transform="'translate(50,20)'"><chart-axis orient="Left" :scale=y :title=axisnamey /></g>
          <g :transform="chartTransform">

          <g :transform="'translate(0,' + 470 +')'"><chart-axis orient="Bottom" :scale=x :title=axisnamex /></g>
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
      random_data: d3.range(1000).map(d3.randomBates(10)),
      width: 960,
      height: 500,
      truew: 1000, //real width of the main svg
      trueh: 540,
      margin: {top: 20, right: 30, bottom: 30, left: 50},
      axisnamey: 'y-axis',
      axisnamex: 'x-axis'
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
      return d3.scaleLinear().domain([0, d3.max(bins, function (d) { return d.length })]).range([height-30, 0]) //range controls the length of the axis. -30 so that the rectangles shift up
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
      return d3.axisBottom(x)
    },
    contentHeight () {
      let height = this.height
      let margin = this.margin
      return height - margin.top - margin.bottom
    },
    chartTransform () {
      let margin = this.margin
      let newtopmargin = margin.top

      return 'translate(' + margin.left + ',' + newtopmargin + ')'
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
```
