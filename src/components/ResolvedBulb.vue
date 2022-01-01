<template>
  <span class="dot" :style="colorStyle"></span>
</template>

<script>
export default {
  name: "ResolvedBulb",
  computed: {
    colorStyle() {
      return {
        'background-color': this.color

      }
    }
  },


  props: {
    color: {
      type: String,
    }

  }}
</script>

<style scoped>
.dot {
  background-color: cornsilk;
  border: solid grey;
  margin: 3px;
  height: 25px;
  width: 25px;
  border-radius: 50%;
  display: inline-block;
}
</style>