<template>
  <button class="btn" :style="{ 'background-color': color }" @click="onClick()">
    <!--veya v-on:click veya @click-->
    {{ text }}
  </button>
</template>

<script>
export default {
  name: 'Button',
  props: {
    text: String,
    color: String,
  },
  methods: {
    onClick() {
      // console.log('cliccked');
    },
  },
};
</script>

<style>
button {
  background-color: color;
}
</style>
