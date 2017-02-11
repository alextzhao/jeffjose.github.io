<template>
    <div class="graph panel">
      <h1>{{ title }}</h1>
      <slider :props="props"></slider>
      <curve :values="values" :width="width" :height="height" :title="title"></curve>
    </div>
</template>

<script lang="coffee">

Slider = require './Slider'
Curve = require './Curve'

module.exports =
    name: 'graph'
    props: ['title', 'props', 'values', 'width', 'height']
    components: {Slider, Curve}
</script>

<style lang="less">
@BGColor: white;
.panel {
    background-color: @BGColor;
    &:hover {
        background-color: darken(@BGColor, 5%);
    }
}
</style>
