<template>
  <div id="app">
      <div :style="themeColor" class="header">
          <div class="title">INTERACTIVE STATISTICAL DISTRIBUTIONS</div>
          <div class="madeby">
              <a href="http://jeffreyjose.com">Made by <span>Jeffrey Jose</span></a>
          </div>
      </div>

      <graph title="Beta Distribution" :props="beta" :dist="jstat.jStat.beta" width=900 height=400 :samples="[0, 1, 0.01]"></graph>

      <graph title="Gamma Distribution" :props="gamma" :dist="jstat.jStat.gamma" width="900" height="400" :samples="[0, 1, 0.01]"></graph>

      <graph title="Normal Distribution" :props="normal" :dist="jstat.jStat.normal" width="900" height="400" :samples="[0, 1, 0.01]"></graph>

      <graph title="Weibull Distribution" :props="weibull" :dist="jstat.jStat.weibull" width="900" height="400" :samples="[0, 1, 0.01]"></graph>

      <graph title="Poisson Distribution" :props="poisson" :dist="jstat.jStat.poisson" width="900" height="400" :samples="[0, 20, 1]"></graph>

      <graph title="Negative Binomial Distribution" :props="negbin" :dist="jstat.jStat.negbin" width="900" height="400":samples="[0, 100, 1]"></graph>

      <graph title="Binomial Distribution" :props="bin" :dist="jstat.jStat.binomial" width="900" height="400":samples="[0, 100, 1]"></graph>

      <graph title="Chi-squared Distribution" :props="chisq" :dist="jstat.jStat.chisquare" width="900" height="400" :samples="[0, 100, 1]"></graph>

      <graph title="Exponential Distribution" :props="exp" :dist="jstat.jStat.exponential" width="900" height="400" :samples="[0, 100, 1]"></graph>

      <div :style="themeColor" class="footer">
          <div class="content">
              <p>Made by <a href="http://jeffreyjose.com">Jeffrey Jose</a>.</p>
              <p>Statistical Distributions come from the excellent <a href="http://jstat.github.io/">jStat</a></p>
              <p>Built using <a href="https://vuejs.org">VueJs</a> by <a href="http://evanyou.me/">Evan You</a> and <a href="https://d3js.org/">d3js</a> by <a href="https://bost.ocks.org/mike/">Mike Bostock</a>.</p>
              <p>Proudly hosted on <a href="https://github.com/jeffjose/jeffjose.github.io/tree/master/stats">Github</a>.</p>
          </div>
      </div>

  </div>
</template>

<script lang="coffee">

Graph = require './components/Graph'
Utils = require './utils/Utils'

randomcolor = require 'randomcolor'

jstat = require 'jstat'

module.exports =
  name: 'app',
  components: {Graph}
  mixins: [Utils]
  methods:
      getColor: () -> randomcolor('luminosity': 'dark')
  data: () ->
    jstat: jstat
    themeColor: {backgroundColor: @getColor()}
    beta: [
        mode: 'pdf'
        area: false
        dots: false
        p: [
            @createParam('alpha', 1.9, 0, 20, 0.01)
            @createParam('beta', .73, 0, 10, 0.01)]
    ]
    gamma: [
        mode: 'pdf'
        area: false
        dots: false
        p: [
            @createParam('shape', .5, 0, 20, 0.01)
            @createParam('scale', 1, 0, 20, 0.01)]
    ]
    normal: [
        mode: 'pdf'
        area: false
        dots: false
        p: [
            @createParam('mean', .5, 0, 1, 0.01)
            @createParam('std', .1, 0, 1, 0.01)]
    ]
    weibull: [
        mode: 'pdf'
        area: false
        dots: false
        p: [
            @createParam('scale', .5, 0, 20, 0.01)
            @createParam('shape', 1, 0, 20, 0.01)]
    ]
    poisson: [
        mode: 'pdf'
        area: false
        dots: false
        discrete: true
        connect: false
        p: [
            @createParam('lambda', 5.5, 0, 20, 1)]
    ]
    negbin: [
        mode: 'pdf'
        area: false
        dots: false
        discrete: true
        connect: false
        p: [
            @createParam('n', 4, 0, 10, .1)
            @createParam('p', .3, 0, 1, 0.01, true)]
    ]
    bin: [
        mode: 'pdf'
        area: false
        dots: false
        discrete: true
        connect: false
        p: [
            @createParam('n', 10, 0, 100, 1)
            @createParam('p', .8, 0, 1, 0.01, true)]
    ]
    chisq: [
        mode: 'pdf'
        area: false
        dots: false
        p: [
            @createParam('df', 3, 0, 40, 1)]
    ]
    exp: [
        mode: 'pdf'
        area: false
        dots: false
        p: [
            @createParam('rate', .4, 0, 1, .01)]
    ]
</script>

<style scoped lang="less">
@textColor: rgba(0, 0, 0, 0.8);
@textColor: rgba(255, 255, 255, 0.5);

    @import url('https://fonts.googleapis.com/css?family=Open+Sans');

    #app {
      font-family: 'Open Sans', 'Avenir', Helvetica, Arial, sans-serif;
      -webkit-font-smoothing: antialiased;
      -moz-osx-font-smoothing: grayscale;
      text-align: center;
      color: #2c3e50;
    }

    // For material icons
    // .. from http://google.github.io/material-design-icons/

    @import url('https://fonts.googleapis.com/css?family=Poiret+One|Comfortaa|Material+Icons');

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



    .header {
        height: 60px;
        width: 100%;
        background-color: red;
        display: flex;
        flex-flow: row nowrap;

        .title {
            color: white;
            font-size: 24px;
            padding: 10px;
            margin: auto 0px;
            font-family: 'Poiret One', sans-serif;
            text-transform: capitalize;
            font-weight: bold;
        }

        .madeby {
            transition: all ease-in-out 100ms;
            margin: auto 10px auto auto;
            color: @textColor;
            line-height: 20px;

            span, a {
                color: @textColor;
                text-decoration: none;
                transition: all ease-in-out 100ms;
            }

            &:hover {

                color: white;

                span, a {
                    color: white;
                }
            }
        }


    }

    .footer {
        height: 150px;
        display: flex;

        .content {
            margin: auto;
            width: 80%;
            height: 200px;
            color: @textColor;
            line-height: 15px;

            a {
                transition: all ease-in-out 100ms;
                text-decoration: none;
                color: @textColor;
                border-bottom: 1px solid @textColor;

                &:hover {
                    color: white;
                    border-bottom: 1px solid white;
                }
            }
        }
    }
</style>
