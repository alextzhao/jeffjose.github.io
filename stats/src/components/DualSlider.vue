<template>
    <div class="dual-slider" :style="{'background-color': color}">
        <slider v-model="props[0].data" :label="props[0].name" :min=props[0].min :max=props[0].max :step=props[0].step :bounded=props[0].bounded></slider>
        <slider v-model="props[1].data" :label="props[1].name" :min=props[1].min :max=props[1].max :step=props[1].step :bounded=props[1].bounded></slider>
    </div>
</template>

<script lang="coffee">

Slider = require './Slider'
Utils   = require '../utils/Utils'

module.exports =
  name: 'dual-slider'
  mixins: [Utils]
  components: {Slider}
  props: ['props', 'color']
</script>

<style scoped lang="less">
.dual-slider {
    display: inline-block
}
</style>
