<template>
  <histogram />
</template>
<script>
import Histogram from './Histogram.vue'
import LineGraph from './LineGraph.vue'
import ScatterPlot from './ScatterPlot.vue'
export default {
  name: 'AllCharts',
  components: {
    'histogram': Histogram,
    'line-graph': LineGraph,
    'scatter-plot': ScatterPlot
  }
}
</script>
