<template>
    <div class="wrapper" @wheel.prevent="wheel($event.wheelDelta)">
      <div class="left">
          <span class="label"><p>{{label}}</p></span>
          <input class="slider" type="range" v-bind:value.number="value" v-on:input="update($event.target.value)" :min="min" :max="max" :step="step">
      </div>

      <div class="right">
          <span class="text">
              <input class="value" v-bind:value.number="value" @wheel="wheel($event.wheelDelta)" v-on:input="update($event.target.value)" :min="min" :max="max" :step="step">
          </span>
          <span class="buttons">
              <input class="plus" type="button" value="+" @click.prevent="increment"></input>
              <input class="minus" type="button" value="-" @click.prevent="decrement"></input>
          </span>
     </div>
    </div>
</template>

<script lang="coffee">

d3 = require 'd3'
_  = require 'lodash'

module.exports =
    name: 'slider-base'
    props: ['label', 'value', 'max', 'min', 'step', 'bounded']
    methods:
        update: (value) ->

            _number = Number(value)

            if _.isNaN(_number)
                number = 0
            else
                number = _number

            if @bounded
                if number > @max
                    number = _.min([@max, number])
                else if number < @min
                    number = _.max([@min, number])

            @$emit('input', number)

        increment: () ->
            _newValue = Number(@value) + Number(@step)
            newValue = Math.round(100*_newValue) / 100

            @update(newValue)

        decrement: () ->
            _newValue = Number(@value) - Number(@step)
            newValue = Math.round(100*_newValue) / 100

            @update(newValue)

        wheel: (direction) ->

            if direction > 0
                @increment()
            if direction < 0
                @decrement()

</script>

<style lang="less">

@textColorDark: #333333;
@textColorLight: #FFFFFF;

.wrapper {
    width: 200px;
    margin: 10px;
    padding: 10px 0px;
    border: 1px solid rgba(0, 0, 0, 0);

    &:hover {
        border: 1px solid rgba(255, 255, 255, 0.3);
    }
}


.left {
    float: left;
    height: 20px;

    .label {
        font-size: 10px;
        display: inline-block;
        height: 100%;
        vertical-align: middle;
        text-align: center;
        color: @textColorLight;
        float: left;
        font-size: 10px;
        padding: 0px 3px;
        width: 40px;

        p {
            float: right;
            margin: 0px;

            margin-top: 3px;
        }

    }
    .slider {
        height: 20px;
        margin: 0px;

        width: 70px;

    }
}

.right {

    .text {
        float: left;

        input {
            float: left;
            border: none;
            padding: 0px;
            color: @textColorDark;
            width: 26px;
            height: 20px;

            font-size: 10px;
            line-height: 0px;
            vertical-align: middle;
            text-align: center;

            margin: 0px 2px;

        }
    }

    .buttons {
        input {
            width: 10px;
            height: 10px;
            line-height: 0px;
            border: none;
            padding: 0px;
            display: block;
            font-size: 10px;

            color: @textColorDark;

            &:hover {
                background-color: darken(white, 40%);
            }
        }

        .minus {
        }

        .plus {
        }
    }
}

</style>
