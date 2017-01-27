<template>
  <div id="app">
      {{alpha}}
      </br>
      <input type="range" v-model.number="alpha" min=0 max=20 step="0.1">
      </br>
      {{beta}}
      </br>
      <input type="range" v-model.number="beta" min=0 max=20 step="0.1">
    <curve :values="values" width="800" height="400" title="Beta Distribution"></curve>
  </div>
</template>

<script lang="coffee">

Curve = require './components/Curve'

jstat = require 'jstat'

#[alpha, beta] = [1, 1]
#[alpha, beta] = [.7, 5]
#[alpha, beta] = [5, 73]

module.exports =
  name: 'app',
  components: {Curve}
  data: () ->
    alpha: 1.53
    beta: .73

  computed:
      values: () ->
          (jstat.jStat.beta.pdf(x/100, @alpha, @beta) for x in [0...100])

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
