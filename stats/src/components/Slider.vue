<template>
    <div>
      {{value}}
      </br>
      <input type="range" v-bind:value="value" v-on:input="update($event.target.value)" min=0 max=20 step="0.01">
      </br>
    </div>
</template>

<script lang="coffee">
module.exports =
    name: 'slider'
    props: ['value', 'range']
    watch:
        range: () -> [@min, @max, @step] = range

    methods:
        update: (value) -> @$emit('input', Number(value))


</scripts>

<style lang="less">
</style>
