<script lang="coffee">

d3       = require 'd3'
d3Colors = require 'd3-scale-chromatic'

SCHEME = d3Colors.schemeDark2

module.exports =

    data: () ->
        colors: d3.scaleOrdinal(SCHEME)
        uuid: @getUuid() for i in [0...10]
    methods:
        getUuid: () ->

          s4 = () ->
            Math.floor((1 + Math.random()) * 0x10000)
              .toString(16)
              .substring(1)

          "x#{s4()}"

        createParam: (name, data, min, max, step, bounded=false) ->
            param =
                name: name
                data: data
                min: min
                max: max
                step: step
                bounded: bounded

            return param

</script>
