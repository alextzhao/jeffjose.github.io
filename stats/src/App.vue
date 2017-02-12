<template>
  <div id="app">
      <graph title="Beta Distribution" :props="beta" :dist="jstat.jStat.beta" width=800 height=400 :samples="[0, 1, 0.01]"></graph>

      <graph title="Gamma Distribution" :props="gamma" :dist="jstat.jStat.gamma" width="800" height="400" :samples="[0, 1, 0.01]"></graph>

      <graph title="Normal Distribution" :props="normal" :dist="jstat.jStat.normal" width="800" height="400" :samples="[0, 1, 0.01]"></graph>

      <graph title="Poisson Distribution" :props="poisson" :dist="jstat.jStat.poisson" width="800" height="400" discrete="true" :samples="[0, 20, 1]"></graph>

      <graph title="Negative Binomial Distribution" :props="negbin" :dist="jstat.jStat.negbin" width="800" height="400" discrete="true" :samples="[0, 100, 1]"></graph>

      <graph title="Binomial Distribution" :props="bin" :dist="jstat.jStat.binomial" width="800" height="400" discrete="true" :samples="[0, 100, 1]"></graph>

      <graph title="Chi-squared Distribution" :props="chisq" :dist="jstat.jStat.chisquare" width="800" height="400" :samples="[0, 100, 1]"></graph>

      <graph title="Exponential Distribution" :props="exp" :dist="jstat.jStat.exponential" width="800" height="400" :samples="[0, 100, 1]"></graph>

  </div>
</template>

<script lang="coffee">

Graph = require './components/Graph'
Utils = require './utils/Utils'

jstat = require 'jstat'

module.exports =
  name: 'app',
  components: {Graph}
  mixins: [Utils]
  data: () ->
    jstat: jstat
    beta: [
        mode: 'pdf'
        area: false
        p: [
            @createParam('alpha', 1.9, 0, 20, 0.01)
            @createParam('beta', .73, 0, 10, 0.01)]
    ]
    gamma: [
        mode: 'pdf'
        area: false
        p: [
            @createParam('shape', .5, 0, 1, 0.01)
            @createParam('scale', 1, 0, 20, 0.01)]
    ]
    normal: [
        mode: 'pdf'
        area: false
        p: [
            @createParam('mean', .5, 0, 1, 0.01)
            @createParam('std', .1, 0, 1, 0.01)]
    ]
    poisson: [
        mode: 'pdf'
        area: false
        p: [
            @createParam('lambda', 5.5, 0, 20, 1)]
    ]
    negbin: [
        mode: 'pdf'
        area: false
        p: [
            @createParam('n', 4, 0, 10, 1)
            @createParam('p', .3, 0, 1, 0.01, true)]
    ]
    bin: [
        mode: 'pdf'
        area: false
        p: [
            @createParam('n', 10, 0, 100, 1)
            @createParam('p', .8, 0, 1, 0.01, true)]
    ]
    chisq: [
        mode: 'pdf'
        area: false
        p: [
            @createParam('df', 3, 0, 40, 1)]
    ]
    exp: [
        mode: 'pdf'
        area: false
        p: [
            @createParam('rate', .4, 0, 1, .01)]
    ]
</script>

<style lang="less">

    @import url('https://fonts.googleapis.com/css?family=Open+Sans');

    #app {
      font-family: 'Open Sans', 'Avenir', Helvetica, Arial, sans-serif;
      -webkit-font-smoothing: antialiased;
      -moz-osx-font-smoothing: grayscale;
      text-align: center;
      color: #2c3e50;
      margin-top: 60px;
    }

    // For material icons
    // .. from http://google.github.io/material-design-icons/

    @import url('https://fonts.googleapis.com/css?family=Material+Icons');

    .material-icons {
      font-family: 'Material Icons';
      font-weight: normal;
      font-style: normal;
      font-size: 24px;  /* Preferred icon size */
      display: inline-block;
      line-height: 1;
      text-transform: none;
      letter-spacing: normal;
      word-wrap: normal;
      white-space: nowrap;
      direction: ltr;

      /* Support for all WebKit browsers. */
      -webkit-font-smoothing: antialiased;
      /* Support for Safari and Chrome. */
      text-rendering: optimizeLegibility;

      /* Support for Firefox. */
      -moz-osx-font-smoothing: grayscale;

      /* Support for IE. */
      font-feature-settings: 'liga';
    }
</style>
