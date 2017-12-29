<template>
  <g>
  <g ref="axis"></g>
  </g>
</template>

<script>
import * as d3 from 'd3'
export default {
  name: 'ChartAxis',
  props: {
    'orient': { // Left, Right, Top, Bottom
      type: String,
      required: true
    },
    'scale': { // D3 scale function, like d3.scaleLinear
      type: Function,
      required: true
    }
  },
  methods: {
    _draw () {
      // Use D3 to draw the axis to the 'g' node
      // console.log(`*** Drawing ${this.orient} axis: range ${this.scale.range()}, domain ${this.scale.domain()}`);
      d3.select(this.$refs.axis).call(d3['axis' + this.orient](this.scale))
    }
  },
  mounted: function () {
    // Draw the axis for the first time
    this._draw()
  }
}
</script>
