<template>
  <div id="app">
    <vline></vline>
  </div>
</template>

<script lang="coffee">

Vline = require './components/Line'

module.exports =
  name: 'app',
  components: { Vline }

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
