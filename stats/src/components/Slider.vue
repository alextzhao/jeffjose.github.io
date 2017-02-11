<template>
    <div class="slider" :style="{'background-color': color}">
        <slider-base v-for="prop, i in props" v-model="prop.data" :label="prop.name" :min=prop.min :max=prop.max :step=prop.step :bounded=prop.bounded></slider-base>
    </div>
</template>

<script lang="coffee">

SliderBase = require './SliderBase'
Utils   = require '../utils/Utils'

module.exports =
  name: 'slider'
  mixins: [Utils]
  components: {SliderBase}
  props: ['props', 'color']
</script>

<style scoped lang="less">
.slider {
    display: inline-block
}
</style>
