<template>
  <div class="header">
    <span id="logo">2048</span>
    <span id="userScore" ref="score">得分：{{ this.$parent.score }}</span>
  </div>
</template>

<script>
export default {
  name: 'Header1',
  props: {
    score: Number
  }
}
</script>

<style scoped>
  @import '../style/main.css';
</style>