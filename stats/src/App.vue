<template>
  <div id="app">
    <curve></curve>
    <vline></vline>
  </div>
</template>

<script lang="coffee">

Vline = require './components/Line'
Curve = require './components/Curve'

module.exports =
  name: 'app',
  components: { Vline, Curve }

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
