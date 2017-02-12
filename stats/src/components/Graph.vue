<template>
    <div class="graph panel">
      <h1>{{ title }}</h1>
      <slider :stats="stats" :props="props"></slider>
      <curve :values="values" :width="width" :height="height" :title="title" :points="discrete"></curve>
    </div>
</template>

<script lang="coffee">

_ = require 'lodash'

Utils = require '../utils/Utils'

Slider = require './Slider'
Curve = require './Curve'

module.exports =
    name: 'graph'
    mixins: ['Utils']
    props: ['title', 'props', 'dist', 'samples', 'width', 'height', 'discrete']
    components: {Slider, Curve}
    methods:
        getSamples: () -> _.range(@samples[0], @samples[1], @samples[2])
        round: (val) -> Math.round(100 * val) / 100

    computed:
        values: () ->

            samples = @getSamples()

            # Check the first props to see whether this is a 1 param, 2 param or 3 param distribution
            if @props[0].length is 1
                values = (@dist.pdf(x, params[0].data) for x in samples for params in @props)
            else if @props[0].length is 2
                values = (@dist.pdf(x, params[0].data, params[1].data) for x in samples for params in @props)
            else if @props[0].length is 3
                values = (@dist.pdf(x, params[0].data, params[1].data, params[2].data) for x in samples for params in @props)

            return values

        stats: () ->

            # Check the first props to see whether this is a 1 param, 2 param or 3 param distribution
            if @props[0].length is 1

                stats = ({
                        mean:     @round(@dist.mean?(params[0].data))
                        median:   @round(@dist.median?(params[0].data))
                        mode:     @round(@dist.mode?(params[0].data))
                        variance: @round(@dist.variance?(params[0].data))
                } for params in @props)

            else if @props[0].length is 2
                stats = ({
                        mean:     @round(@dist.mean?(params[0].data, params[1].data))
                        median:   @round(@dist.median?(params[0].data, params[1].data))
                        mode:     @round(@dist.mode?(params[0].data, params[1].data))
                        variance: @round(@dist.variance?(params[0].data, params[1].data))
                } for params in @props)
            else if @props[0].length is 3
                stats = ({
                        mean:     @round(@dist.mean?(params[0].data, params[1].data, params[2].data))
                        median:   @round(@dist.median?(params[0].data, params[1].data, params[2].data))
                        mode:     @round(@dist.mode?(params[0].data, params[1].data, params[2].data))
                        variance: @round(@dist.variance?(params[0].data, params[1].data, params[2].data))
                } for params in @props)

            return stats

</script>

<style lang="less">
@BGColor: white;
.panel {
    background-color: @BGColor;
    &:hover {
        background-color: darken(@BGColor, 5%);
    }
}
</style>
