<template>
  <div id="app">
    <img src="./assets/logo.png">
    <line></line>
  </div>
</template>

<script lang="coffee">
Line = require './components/Line'

module.exports =
  name: 'app',
  components: { Line }

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
