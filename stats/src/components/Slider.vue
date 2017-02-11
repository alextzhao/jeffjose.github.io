<template>
    <div class="slider-wrapper">
        <div v-for="prop, i in props" class="slider" :style="{'background-color': colors(i)}">
            <slider-base v-for="p in prop" v-model="p.data" :label="p.name" :min=p.min :max=p.max :step=p.step :bounded=p.bounded></slider-base>
        </div>
        <div @click="addAnother" class="add-another">
            <div class="plus">+</div>
            <div class="distribution">Distribution</div>
        </div>
    </div>
</template>

<script lang="coffee">

SliderBase = require './SliderBase'
Utils   = require '../utils/Utils'

module.exports =
  name: 'slider'
  mixins: [Utils]
  components: {SliderBase}
  props: ['props']
  methods:
    addAnother: () ->
        prop = _.cloneDeep(@props[0])

        # Create a random "data" attribute
        #  if the prop.step == 1, then its an integer
        #  else if prop.step != 1, then its a float
        for p in prop
            _data = _.random(p.min, p.max, p.step isnt 1)
            data = Math.round(100*_data) / 100
            p.data = data

        @props.push(prop)
</script>

<style scoped lang="less">

@addAnotherBGColor: #E0E0E0;

.slider-wrapper {
    display: table;

    .slider {
        display: inline-block;
    }

    .add-another {
        display: table-cell;
        width: 200px;
        background-color: @addAnotherBGColor;
        height: 100%;
        vertical-align: middle;
        cursor: pointer;

        color: darken(@addAnotherBGColor, 50%);
        line-height: 24px;

        .plus {
            font-size: 32px;
        }

        .distribution {
            font-size: 16px;
        }

        &:hover {
            background-color: darken(@addAnotherBGColor, 5%);
        }
    }

}
</style>
