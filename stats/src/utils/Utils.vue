<script lang="coffee">

d3       = require 'd3'
d3Colors = require 'd3-scale-chromatic'

SCHEME = d3Colors.schemeDark2

module.exports =

    data: () ->
        colors: d3.scaleOrdinal(SCHEME)
        uuid: @getUuid() for i in [0...10]
    computed:
        #colors: () -> d3.scaleOrdinal(d3Colors.schemeDark2)
        a: () -> 'foo'
    methods:
        getUuid: () ->

          s4 = () ->
            Math.floor((1 + Math.random()) * 0x10000)
              .toString(16)
              .substring(1)

          "x#{s4()}"

</script>
