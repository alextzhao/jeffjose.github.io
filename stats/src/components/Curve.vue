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
_ = require 'underscore'

INFINITY = 100
YMAX = 4

module.exports =

  name: 'curve',
  props: {
    title: String
    height: String
    width: String,
    values: Array,
    points:
        default: false
    }
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
         values.map (x) ->

            if (x is Infinity) or (x is -Infinity)
                return INFINITY
            else if _.isNaN(x)
                return 0
            else
                return x



    draw: () ->
      @setupSVG()
      @scale = @setupScales()
      @drawGraph()
      @drawAxis()

    redraw: () ->
      @drawGraph()

    setupSVG: () ->
        @dims =
            width: @svg.attr('width') - @margin.left - @margin.right
            height: @svg.attr('height') - @margin.top - @margin.bottom

        @curve
          .attr('transform', "translate(#{@margin.left}, #{@margin.top})")

    setupScales: () ->
        x = d3.scaleLinear().range([0, @dims.width])
        y = d3.scaleLinear().range([@dims.height, 0])

        x.domain(d3.extent(@data, (d, i) => i/@data.length))

        # Hardcoding YMAX
        #y.domain([0, YMAX])
        y.domain([0, d3.min([YMAX, d3.max(@data, (d) => d)])])

        { x, y }

    drawAxis: () ->

        @curve.append('g')
            .attr('transform', "translate(0, #{@dims.height})")
            .attr('class', 'axis xaxis')
            .call(d3.axisBottom(@scale.x))

        @curve.append('g')
            .attr('class', 'axis yaxis')
            .call(d3.axisLeft(@scale.y).ticks(5))


    drawGraph: () ->
        @drawPath()

        if @points
            @drawPoints()

    drawPoints: () ->
        @curve.selectAll('circle.points').remove()

        circles = @curve.selectAll('circle')
            .data(@data, (d, i) -> d)
            .enter().append('circle')
                .attr('class', 'points')
                .attr('r', 5)
                .attr('cx', (d, i) => @scale.x(i/@data.length))
                .attr('cy', (d, i) => @scale.y(d))

    drawPath: () ->
        path = d3.line()
            .x((d, i) => @scale.x(i/@data.length))
            .y((d) => @scale.y(d))

        @line = path(@data)

        # If points are being displayed, path needs to be transparent
        @curve.selectAll('path.line')
            .classed('transparent', @points)

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="less">

@axisColor: #9E9E9E;
//@lineColor: #E91E63;
@lineColor: #FF7043;

path.line {
    stroke-width: 2;
    stroke: @lineColor;
    fill: none;
}

g.axis {
    path, line {
        stroke: @axisColor;
    }

    text {
        fill: @axisColor;
    }

    &.yaxis .tick * , &.xaxis .tick * {
        stroke: none;
    }
}

.transparent {
    stroke-opacity: 0.2;
}

circle.points {
    fill: @lineColor;
    //fill-opacity: 0.6;
}
</style>
