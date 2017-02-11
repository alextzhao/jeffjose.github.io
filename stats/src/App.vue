<template>
  <div id="app">
      <div class="panel">
          <dual-slider v-for="param, i in beta" :color="colors(i)" :props="param"></dual-slider>
          <button @click="addAnotherSlider(beta)">Add Another</button>
          <curve :values="betavalues" width="800" height="400" title="Beta Distribution"></curve>
      </div>

      <!--
      <div class="panel">
          <slider v-model="gamma.shape" label="shape" min=0 max=1 step=0.01></slider>
          <slider v-model="gamma.scale" label="scale" min=0 max=20 step=0.01></slider>
          <curve :values="gammavalues" width="800" height="400" title="Gamma Distribution"></curve>
      </div>

      <div class="panel">
          <slider v-model="normal.mean" label="mean" min=0 max=1 step=0.01></slider>
          <slider v-model="normal.std"  label="std" min=0 max=1 step=0.01></slider>
          <curve :values="normalvalues" width="800" height="400" title="Normal Distribution"></curve>
      </div>

      <div class="panel">
          <slider v-model="poisson.lambda" label="lambda" min=0 max=10 step=0.01></slider>
          <curve :values="poissonvalues" width="800" height="400" title="Poisson Distribution" :points="true"></curve>
      </div>

      -->
      <div class="panel">
          <dual-slider v-for="param, i in negbin" :color="colors(i)" :props="param"></dual-slider>
          <button @click="addAnotherSlider(negbin)">Add Another</button>
          <curve :values="negbinvalues" width="800" height="400" title="Negative Binomial Distribution" :points="true"></curve>
      </div>

      <div class="panel">
          <dual-slider v-for="param, i in bin" :color="colors(i)" :props="param"></dual-slider>
          <button @click="addAnotherSlider(bin)">Add Another</button>
          <curve :values="binvalues" width="800" height="400" title="Binomial Distribution" :points="true"></curve>
      </div>

  </div>
</template>

<script lang="coffee">

Curve = require './components/Curve'
Slider = require './components/Slider'
DualSlider = require './components/DualSlider'

Utils = require './utils/Utils'

jstat = require 'jstat'
_ = require 'lodash'

#[alpha, beta] = [1, 1]
#[alpha, beta] = [.7, 5]
#[alpha, beta] = [5, 73]

module.exports =
  name: 'app',
  components: {Curve, Slider, DualSlider}
  mixins: [Utils]
  data: () ->
    gamma:
        shape:.5
        scale: 1
    normal:
        mean: .5
        std: .10
    poisson:
        lambda: 5.5
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
          (jstat.jStat.gamma.pdf(x/100, @gamma.shape, @gamma.scale) for x in [0...100])
      normalvalues: () ->
          (jstat.jStat.normal.pdf(x/100, @normal.mean, @normal.std) for x in [0...100])
      poissonvalues: () ->
          (jstat.jStat.poisson.pdf(x, @poisson.lambda) for x in [0...20])
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
