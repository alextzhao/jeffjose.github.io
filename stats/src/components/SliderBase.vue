<template>
    <div :class='{noPointerEvents: readonly}'class="wrapper" @wheel.prevent="wheel($event.wheelDelta)">
      <div class="left">
          <span class="label"><p>{{label}}</p></span>
          <input class="slider" type="range" v-bind:value.number="value" v-on:input="update($event.target.value)" :min="min" :max="max" :step="step">
      </div>

      <div class="right">
          <span class="buttons">
              <input class="plus" type="button" value="+" @click.prevent="increment"></input>
              <input class="minus" type="button" value="-" @click.prevent="decrement"></input>
          </span>

          <span class="text">
              <input class="value" v-bind:value.number="value" @wheel="wheel($event.wheelDelta)" v-on:input="update($event.target.value)" :min="min" :max="max" :step="step">
          </span>

     </div>
    </div>
</template>

<script lang="coffee">

d3 = require 'd3'
_  = require 'lodash'

Utils = require '../utils/Utils'

module.exports =
    name: 'slider-base'
    props: ['label', 'value', 'max', 'min', 'step', 'bounded', 'readonly']
    mixins: ['Utils']
    methods:
        round: (val) -> Math.round(100 * val) / 100
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
            newValue = @round(Number(@value) + Number(@step))

            @update(newValue)

        decrement: () ->
            newValue = @round(Number(@value) - Number(@step))

            @update(newValue)

        wheel: (direction) ->

            if direction > 0
                @increment()
            if direction < 0
                @decrement()

</script>

<style scoped lang="less">

@textColorDark: #333333;
@textColorLight: #FFFFFF;

.wrapper {
    height: 20px;
    width: 200px;
    margin: 3px;
    padding: 5px 0px;
    border: 1px solid rgba(0, 0, 0, 0);
    transition: all ease-in-out 100ms;

    align-self: center;

    &:hover {
        background-color: rgba(255, 255, 255, 0.2);
    }
}


.left {
    float: left;
    height: 20px;
    margin-right: 10px;
    margin-left: 5px;

    .label {
        display: inline-block;
        height: 100%;
        vertical-align: middle;
        text-align: center;
        color: fade(@textColorLight, 80%);
        float: left;
        font-size: 12px;
        padding: 0px 5px;
        width: 40px;
        user-select: none;

        p {
            float: right;
            margin: 0px;

            margin-top: 1px;
        }

    }
    .slider {
        height: 20px;
        margin: 0px;

        width: 70px;
        opacity: 0.9;

        &:hover {
            opacity: 1;
        }

    }
}

.right {

    .text {

        input {
            float: left;
            border: none;
            padding: 0px;
            color: @textColorLight;
            background-color: rgba(0, 0, 0, 0);
            width: 50px;
            height: 20px;

            font-size: 14px;
            line-height: 0px;
            vertical-align: middle;
            text-align: center;

            margin: 0px 2px;

            &:hover {
                color: @textColorLight;
                border-bottom: 1px solid @textColorLight;
            }

            &:focus {
                outline: none;
            }

            &::selection {
                background-color: rgba(0, 0, 0, 0.3);
                padding: 10px;
            }

        }
    }

    .buttons {
        float: left;
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
                background-color: darken(white, 20%);
            }

            &:focus {
                outline: none;
            }

        }

        .minus {
        }

        .plus {
        }
    }
}

.noPointerEvents {
    pointer-events: none;
    cursor: pointer;

	background: repeating-linear-gradient(
	  45deg,
      rgba(200, 200, 200, 0.6),
      rgba(200, 200, 200, 0.6) 10px,
      rgba(200, 200, 200, 0.1) 10px,
      rgba(200, 200, 200, 0.1) 20px,
	);
}

</style>
