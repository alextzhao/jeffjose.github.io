<template>
  <div id="app">
      <graph title="Beta Distribution" :props="beta" :values="betavalues" width="800" height="400"></graph>

      <graph title="Gamma Distribution" :props="gamma" :values="gammavalues" width="800" height="400"></graph>

      <graph title="Normal Distribution" :props="normal" :values="normalvalues" width="800" height="400"></graph>

      <graph title="Poisson Distribution" :props="poisson" :values="poissonvalues" width="800" height="400"></graph>

      <graph title="Negative Binomial Distribution" :props="negbin" :values="negbinvalues" width="800" height="400"></graph>

      <graph title="Binomial Distribution" :props="bin" :values="binvalues" width="800" height="400"></graph>

  </div>
</template>

<script lang="coffee">

Graph = require './components/Graph'

Utils = require './utils/Utils'

jstat = require 'jstat'
_ = require 'lodash'

#[alpha, beta] = [1, 1]
#[alpha, beta] = [.7, 5]
#[alpha, beta] = [5, 73]

module.exports =
  name: 'app',
  components: {Graph}
  mixins: [Utils]
  data: () ->
    gamma: [
        [
                name: 'shape'
                data: .5
                min: 0
                max: 1
                step: 0.01
        ,
                name: 'scale'
                data: 1
                min: 0
                max: 20
                step: 0.01
        ]
    ]
    normal: [
        [
                name: 'mean'
                data: .5
                min: 0
                max: 1
                step: 0.01
        ,
                name: 'std'
                data: .1
                min: 0
                max: 1
                step: 0.01
        ]
    ]
    poisson: [
        [
                name: 'lambda'
                data: 5.5
                min: 0
                max: 20
                step: 1
        ]
    ]
    negbin: [
        [
                name: 'n'
                data: 4
                min: 0
                max: 10
                step: 1
        ,
                name: 'p'
                data: .3
                min: 0
                max: 1
                step: 0.01
                bounded: true
        ]
    ]
    bin: [
        [
                name: 'n'
                data: 10
                min: 0
                max: 100
                step: 1
        ,
                name: 'p'
                data: .8
                min: 0
                max: 1
                step: 0.01
                bounded: true
        ]
    ]
    beta: [
        [
                name: 'alpha'
                data: 1.53
                min: 0
                max: 20
                step: 0.01
            ,
                name: 'beta'
                data: .73
                min: 0
                max: 10
                step: 0.01
        ]
    ]

  methods:
        addAnotherSlider: (props) ->
            prop = _.cloneDeep(props[0])

            # Create a random "data" attribute
            #  if the prop.step == 1, then its an integer
            #  else if prop.step != 1, then its a float
            for p in prop
                _data = _.random(p.min, p.max, p.step isnt 1)
                data = Math.round(100*_data) / 100
                p.data = data

            props.push(prop)

  computed:
      betavalues: () ->
          (jstat.jStat.beta.pdf(x/100, params[0].data, params[1].data) for x in [0...100] for params in @beta)
      gammavalues: () ->
          x = (jstat.jStat.gamma.pdf(x/100, params[0].data, params[1].data) for x in [0...100] for params in @gamma)
          console.log(x)
          return x
      normalvalues: () ->
          (jstat.jStat.normal.pdf(x/100, params[0].data, params[1].data) for x in [0...100] for params in @normal)
      poissonvalues: () ->
          (jstat.jStat.poisson.pdf(x, params[0].data) for x in [0...20] for params in @poisson)
      negbinvalues: () ->
          (jstat.jStat.negbin.pdf(x, params[0].data, params[1].data) for x in [0...100] for params in @negbin)
      binvalues: () ->
          (jstat.jStat.binomial.pdf(x, params[0].data, params[1].data) for x in [0...100] for params in @bin)

</script>

<style lang="less">

    #app {
      font-family: 'Avenir', Helvetica, Arial, sans-serif;
      -webkit-font-smoothing: antialiased;
      -moz-osx-font-smoothing: grayscale;
      text-align: center;
      color: #2c3e50;
      margin-top: 60px;
    }
</style>
