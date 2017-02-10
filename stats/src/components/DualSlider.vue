<template>
    <div>
        <slider v-model="props[0].data" :label="props[0].name" :min=props[0].min :max=props[0].max :step=props[0].step :bounded=props[0].bounded></slider>
        <slider v-model="props[1].data" :label="props[1].name" :min=props[1].min :max=props[1].max :step=props[1].step :bounded=props[1].bounded></slider>

    </div>
</template>

<script lang="coffee">

Slider = require './Slider'

module.exports =
  name: 'dual-slider'
  components: {Slider}
  props: ['props']

</script>

<style lang="less">
</style>
