<template>
  <div id="app">
      <div class="panel">
          <slider v-model="beta.alpha" label="alpha" min=0 max=20 step=0.01></slider>
          <slider v-model="beta.beta" label="beta" min=0 max=20 step=0.01></slider>
          <curve :values="betavalues" width="800" height="400" title="Beta Distribution"></curve>
      </div>

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

      <div class="panel">
          <slider v-model="negbin.r" label="r" min=0 max=10 step=1></slider>
          <slider v-model="negbin.p" label="p" min=0 max=1 step=0.01></slider>
          <curve :values="negbinvalues" width="800" height="400" title="Negative Binomial Distribution" :points="true"></curve>
      </div>

      <div class="panel">
          <slider v-model="bin.n" label="r" min=0 max=100 step=1></slider>
          <slider v-model="bin.p" label="p" min=0 max=1 step=0.01></slider>
          <curve :values="binvalues" width="800" height="400" title="Binomial Distribution" :points="true"></curve>
      </div>

  </div>
</template>

<script lang="coffee">

Curve = require './components/Curve'
Slider = require './components/Slider'

jstat = require 'jstat'

#[alpha, beta] = [1, 1]
#[alpha, beta] = [.7, 5]
#[alpha, beta] = [5, 73]

module.exports =
  name: 'app',
  components: {Curve, Slider}
  data: () ->
    beta:
        alpha: .53
        beta: .73
    gamma:
        shape:.5
        scale: 1
    normal:
        mean: .5
        std: .10
    poisson:
        lambda: 5.5
    negbin:
        r: 4
        p:.3
    bin:
        n: 100
        p:.8

  computed:
      betavalues: () ->
          jstat.jStat.beta.pdf(x/100, @beta.alpha, @beta.beta) for x in [0...100]
      gammavalues: () ->
          (jstat.jStat.gamma.pdf(x/100, @gamma.shape, @gamma.scale) for x in [0...100])
      normalvalues: () ->
          (jstat.jStat.normal.pdf(x/100, @normal.mean, @normal.std) for x in [0...100])
      poissonvalues: () ->
          (jstat.jStat.poisson.pdf(x, @poisson.lambda) for x in [0...20])
      negbinvalues: () ->
          jstat.jStat.negbin.pdf(x, @negbin.r, @negbin.p) for x in [0...100]
      binvalues: () ->
          jstat.jStat.binomial.pdf(x, @bin.n, @bin.p) for x in [0...100]

</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
