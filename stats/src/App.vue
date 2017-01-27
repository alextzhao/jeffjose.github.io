<template>
  <div id="app">
      <div class="panel">
          <slider v-model="beta.alpha" range="[0, 20, 0.01]"></slider>
          <slider v-model="beta.beta" range="[0, 20, 0.01]"></slider>
          <curve :values="betavalues" width="800" height="400" title="Beta Distribution"></curve>
      </div>

      <div class="panel">
          {{gamma.shape}}
          </br>
          <input type="range" v-model.number="gamma.shape" min=0 max=1 step="0.01">
          </br>
          {{gamma.scale}}
          </br>
          <input type="range" v-model.number="gamma.scale" min=0 max=20 step="0.01">
          <curve :values="gammavalues" width="800" height="400" title="Gamma Distribution"></curve>
      </div>

      <div class="panel">
          {{normal.mean}}
          </br>
          <input type="range" v-model.number="normal.mean" min=0 max=1 step="0.01">
          </br>
          {{normal.std}}
          </br>
          <input type="range" v-model.number="normal.std" min=0 max=1 step="0.01">
          <curve :values="normalvalues" width="800" height="400" title="Normal Distribution"></curve>
      </div>

      <div class="panel">
          {{poisson.lambda}}
          </br>
          <input type="range" v-model.number="poisson.lambda" min=0 max=10 step="0.01">
          <curve :values="poissonvalues" width="800" height="400" title="Poisson Distribution" :points="true"></curve>
      </div>

      <div class="panel">
          {{negbin.r}}
          </br>
          <input type="range" v-model.number="negbin.r" min=0 max=10 step="1">
          </br>
          {{negbin.p}}
          </br>
          <input type="range" v-model.number="negbin.p" min=0 max=1 step="0.01">
          <curve :values="negbinvalues" width="800" height="400" title="Negative Binomial Distribution" :points="true"></curve>
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
        alpha: 1.53
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

  computed:
      betavalues: () ->
          (jstat.jStat.beta.pdf(x/100, @beta.alpha, @beta.beta) for x in [0...100])
      gammavalues: () ->
          (jstat.jStat.gamma.pdf(x/100, @gamma.shape, @gamma.scale) for x in [0...100])
      normalvalues: () ->
          (jstat.jStat.normal.pdf(x/100, @normal.mean, @normal.std) for x in [0...100])
      poissonvalues: () ->
          (jstat.jStat.poisson.pdf(x, @poisson.lambda) for x in [0...20])
      negbinvalues: () ->
          (jstat.jStat.negbin.pdf(x, @negbin.r, @negbin.p) for x in [0...100])

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
