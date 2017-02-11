<template>
    <div>
        <svg :height="height" :width="width">
            <g class="wrapper">
                <path v-for="line, i in lines" :stroke="colors(i)" class="line" :d="line"/>
            </g>
        </svg>
    </div>
</template>

<script lang="coffee">

d3       = require 'd3'
_        = require 'lodash'

Utils   = require '../utils/Utils'

INFINITY = 100
YMAX = 4

module.exports =

  name: 'curve',
  mixins: [Utils]
  props: {
    title: String
    height: String
    width: String,
    values: Array,
    points:
        default: false
    }
  data: () ->
    lines: ''
    data: @massage(@values)
    margin:
        top: 30
        right: 30
        bottom: 30
        left: 40

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

        for value, i in values
            value.map (x) ->

               if (x is Infinity) or (x is -Infinity)
                   return INFINITY
               else if _.isNaN(x)
                   return 0
               else
                   return _.max([x, 0])


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

        # Use the first item to setup x axis
        sampleData = @data[0]
        if @points
            x = d3.scaleLinear().range([0, @dims.width])
            x.domain(d3.extent(sampleData, (d, i) => i))
        else
            x = d3.scaleLinear().range([0, @dims.width])
            x.domain(d3.extent(sampleData, (d, i) => i/sampleData.length))

        y = d3.scaleLinear().range([@dims.height, 0])

        # Hardcoding YMAX
        #y.domain([0, YMAX])
        y.domain([0, d3.min([YMAX, d3.max(sampleData, (d) => d)])])

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

        if @points
            for data, i in @data
                @drawBars(data, i)
        else
            # Drawpath works on all curves in one shot
            @drawPath(@data)

    drawBars: (data, i) ->
        uuid = @uuid[i]

        @curve.selectAll(".bars-wrapper.#{uuid}").remove()
        barWrapper = @curve.append("g")
            .attr('class', "bars-wrapper #{uuid}")

        rects = barWrapper.selectAll('rect')
            .data(data, (d, i) -> d)
            .enter().append('rect')
                .attr('class', "bars #{uuid}")
                .style('fill', @colors(i))
                #.attr('width', @scale.x.bandwidth())
                .attr('width', '2')
                .attr('x', (d, i) => @scale.x(i))
                .attr('y', (d, i) => @scale.y(d))
                .attr('height', (d, i) => @dims.height - @scale.y(d))

    drawPoints: (data) ->
        @curve.selectAll('circle.points').remove()

        circles = @curve.selectAll('circle')
            .data(data, (d, i) -> d)
            .enter().append('circle')
                .attr('class', 'points')
                .attr('r', 5)
                .attr('cx', (d, i) => @scale.x(i/data.length))
                .attr('cy', (d, i) => @scale.y(d))

    drawPath: (datas) ->
        path = d3.line()
            .x((d, i) => @scale.x(i/data.length))
            .y((d) => @scale.y(d))

        @lines = (path(data) for data in datas)

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="less">

@axisColor: #9E9E9E;
//@lineColor: #E91E63;
@lineColor: #FF7043;

path.line {
    stroke-width: 2;
    //stroke: @lineColor;
    fill: none;
}

rect.bars {
    opacity: 1;
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
