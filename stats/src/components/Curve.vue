<template>
    <div>
        <h1>{{ title }}</h1>
        <svg :height="height" :width="width">
            <g class="wrapper">
                <path class="line" :d="line"/>
            </g>
        </svg>
    </div>
</template>

<script lang="coffee">

d3 = require 'd3'

INFINITY = 100
YMAX = 4

module.exports =

  name: 'curve',
  props: ['title', 'height', 'width', 'values']
  data: () ->
    line: ''
    data: @massage(@values)
    margin:
        top: 30
        right: 30
        bottom: 30
        left: 30

  watch:
      values: (values) ->
          @data = @massage(@values)

          @redraw()

  mounted: () ->

      @svg   = d3.select(@$el).select('svg')
      @curve = @svg.select('g.wrapper')

      @draw()

  methods:
    massage: (values) ->
        values.map (x) -> if x is Infinity then INFINITY else x

    draw: () ->
      console.log('draw!')
      @setupSVG()
      @scale = @setupScales()
      @drawPath()
      @drawAxis()

    redraw: () ->
      @drawPath()

    setupSVG: () ->
        @dims =
            width: @svg.attr('width') - @margin.left - @margin.right
            height: @svg.attr('height') - @margin.top - @margin.bottom

        @svg
          .select('g.wrapper')
          .attr('transform', "translate(#{@margin.left}, #{@margin.top})")

    setupScales: () ->
        x = d3.scaleLinear().range([0, @dims.width])
        y = d3.scaleLinear().range([@dims.height, 0])

        x.domain(d3.extent(@data, (d, i) => i/100))

        # Hardcoding YMAX
        #y.domain([0, YMAX])
        y.domain([0, d3.max(@data, (d) => d)])

        { x, y }

    drawAxis: () ->

        @curve.append('g')
            .attr('transform', "translate(0, #{@dims.height})")
            .attr('class', 'axis xaxis')
            .call(d3.axisBottom(@scale.x))

        @curve.append('g')
            .attr('class', 'axis yaxis')
            .call(d3.axisLeft(@scale.y).ticks(5))


    drawPath: () ->
        path = d3.line()
            .x((d, i) => @scale.x(i/100))
            .y((d) => @scale.y(d))

        @line = path(@data)

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="less">

path.line {
    stroke-width: 2;
    stroke: #F44336;
    fill: none;
}

g.axis {
    path, line {
        stroke: #455A64;
    }

    text {
        fill: #455A64;
    }
}
</style>
