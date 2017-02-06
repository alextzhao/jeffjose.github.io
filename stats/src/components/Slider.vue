<template>
    <div class="wrapper">
      <div class="left">
          <span class="label"><p>{{label}}</p></span>
          <input class="slider" type="range" v-bind:value.number="value" v-on:input="update($event.target.value)" :min="min" :max="max" :step="step">
      </div>

      <div class="right">
          <span class="text">
              <input class="value" v-bind:value.number="value" v-on:input="update($event.target.value)" :min="min" :max="max" :step="step">
          </span>
          <span class="buttons">
              <input class="minus" type="button" value="-"></input>
              <input class="plus" type="button" value="+"></input>
          </span>
     </div>
    </div>
</template>

<script lang="coffee">
module.exports =
    name: 'slider'
    props: ['label', 'value', 'max', 'min', 'step']
    methods:
        update: (value) ->
            console.log(Number(value))
            @$emit('input', Number(value))

</scripts>

<style lang="less">

@textColor: #9E9E9E;

.wrapper {
    width: 250px;
    margin-top: 20px;
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
        color: @textColor;
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
            color: @textColor;
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

            color: @textColor;

            &:hover {
                background-color: green;
            }
        }

        .minus {
        }

        .plus {
        }
    }
}

</style>
