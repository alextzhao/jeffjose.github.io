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

jstat = require 'jstat'

module.exports =
  name: 'app',
  components: {Graph}
  data: () ->
    jstat: jstat
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
                name: 'df'
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
