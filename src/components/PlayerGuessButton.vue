<template>
  <button type="button" :class="color" class="btn btn-circle btn-sm"></button>
</template>

<script>
export default {
  name: "PlayerGuessButton",
  props: {
    color: {
      type: String,
      required: true
    }
  }
}
</script>

<style scoped>

.btn-circle.btn-sm {
  margin: 10px;
  width: 50px;
  height: 50px;
  padding: 6px 0px;
  border-radius: 25px;
  font-size: 8px;
  text-align: center;
}
</style>