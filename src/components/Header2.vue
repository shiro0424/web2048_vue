<template>
  <div class="header2">
    <span id="instruction">
      <ul>
        <li>欢迎来到2048线上版</li>
        <li>使用上下左右或WASD键控制</li>
        <li>组合数字并争取得到2048！</li>
      </ul>
    </span>
    <button id="btn" @click="newGame()">新游戏</button>
  </div>
</template>

<script>
export default {
  name: "Header2",
  methods:{
    newGame: function (){
      this.$parent.newGame();
    }
  }
}
</script>

<style scoped>
  @import '../style/main.css';
</style>