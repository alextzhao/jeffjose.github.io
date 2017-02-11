<template>
  <div id="app">
      <graph title="Beta Distribution" :props="beta" :values="betavalues" width="800" height="400"></graph>

      <graph title="Gamma Distribution" :props="gamma" :values="gammavalues" width="800" height="400"></graph>

      <graph title="Normal Distribution" :props="normal" :values="normalvalues" width="800" height="400"></graph>

      <graph title="Poisson Distribution" :props="poisson" :values="poissonvalues" width="800" height="400" discrete="true"></graph>

      <graph title="Negative Binomial Distribution" :props="negbin" :values="negbinvalues" width="800" height="400" discrete="true"></graph>

      <graph title="Binomial Distribution" :props="bin" :values="binvalues" width="800" height="400" discrete="true"></graph>

      <graph title="Chi-squared Distribution" :props="chisq" :values="chisqvalues" width="800" height="400"></graph>

      <graph title="Exponential Distribution" :props="exp" :values="expvalues" width="800" height="400"></graph>

  </div>
</template>

<script lang="coffee">

Graph = require './components/Graph'

jstat = require 'jstat'
_ = require 'lodash'

module.exports =
  name: 'app',
  components: {Graph}
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
                data: 1.5
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
    chisq: [
        [
                name: 'dof'
                data: 3
                min: 0
                max: 40
                step: 1
        ]
    ]
    exp: [
        [
                name: 'rate'
                data: .4
                min: 0
                max: 1
                step: .01
        ]
    ]

  computed:
      betavalues: () ->
          (jstat.jStat.beta.pdf(x/100, params[0].data, params[1].data) for x in [0...100] for params in @beta)
      gammavalues: () ->
          (jstat.jStat.gamma.pdf(x/100, params[0].data, params[1].data) for x in [0...100] for params in @gamma)
      normalvalues: () ->
          (jstat.jStat.normal.pdf(x/100, params[0].data, params[1].data) for x in [0...100] for params in @normal)
      poissonvalues: () ->
          (jstat.jStat.poisson.pdf(x, params[0].data) for x in [0...20] for params in @poisson)
      negbinvalues: () ->
          (jstat.jStat.negbin.pdf(x, params[0].data, params[1].data) for x in [0...100] for params in @negbin)
      binvalues: () ->
          (jstat.jStat.binomial.pdf(x, params[0].data, params[1].data) for x in [0...100] for params in @bin)
      chisqvalues: () ->
          (jstat.jStat.chisquare.pdf(x, params[0].data) for x in [0...50] for params in @chisq)
      expvalues: () ->
          (jstat.jStat.exponential.pdf(x, params[0].data) for x in [0...100] for params in @exp)

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
