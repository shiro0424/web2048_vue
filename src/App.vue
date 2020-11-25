<template>
  <div class="container">
    <Header1/>
    <Header2/>
    <div id="gameContainer">
      <div v-for="(row, i) in board" :key="i" class="gameRow">
        <span v-for="(num, j) in row" :key="j" :style="setStyle(i, j)" class="box">{{ (num === 0) ? "" : num }}</span>
      </div>
    </div>
  </div>
</template>

<script>
import Header1 from './components/Header1.vue'
import Header2 from './components/Header2.vue'

export default {
  name: 'App',
  components: {
    Header1,
    Header2
  },
  data: function() {
    return {
      board: [],
      score: 0,
      isFirst: true
    };
  },
  created: function(){
    this.newGame();
  },
  methods: {
    newGame: function (){
      this.isFirst = true;
      this.initBoard();
      this.generateNum();
      this.generateNum();
      this.addController();
    },
    addScore: function(num){
      this.score += num;
    },
    initBoard: function (){
      for(let i = 0; i < 4; i++){
        this.board[i] = [];
        for(let j = 0; j < 4; j++){
          this.board[i][j] = 0;
        }
      }
    },
    generateNum: function(){
      let isValid = false;
      let row, col;
      while(!isValid){
        row = Math.floor(Math.random() * 4);
        col = Math.floor(Math.random() * 4);
        if(this.board[row][col] === 0){
          isValid = true;
          if(this.isFirst){
            this.board[row][col] = 2;
          }else{
            this.board[row][col] = (Math.floor(Math.random() * 100) < 90) ? 2 : 4;
          }
        }
        this.updateNum(row, col);
      }
    },
    setStyle: function (row, col){
      let fontSize = (this.board[row][col] >= 1024) ? "40px" : "50px";
      return {
        backgroundColor: this.getBoxColor(this.board[row][col]),
        fontSize: fontSize,
      }
    },
    getBoxColor: function (num){
      switch(num){
        case 2:
          return "#eee4da";
        case 4:
          return "#ede0c8";
        case 8:
          return "#f2b179";
        case 16:
          return "#f59563";
        case 32:
          return "#f67c5f";
        case 64:
          return "#f65e3b";
        case 128:
          return "#edcf72";
        case 256:
          return "#edcc61";
        case 512:
          return "#9c0";
        case 1024:
          return "#33b5e5";
        default:
          break;
      }
      return "#CDC1B4";
    },
    updateNum: function (r, c){
      let row = this.board[r];
      this.$set(this.board, r, row);
      let num = row[c];
      this.$set(row, c, num);
    },
    upMove: function (){
      for(let i = 0; i < 4; i++){
        let currCol = this.getCol(i);
        let rowIndex = 0;
        for(let j = 0; j < currCol.length; j++){
          if(j + 1 < currCol.length && currCol[j] === currCol[j + 1]){
            if(currCol[j] * 2 !== 2048){
              this.board[rowIndex][i] = currCol[j] * 2;
              this.updateNum(rowIndex, i);
              j++;
            }
            this.addScore(this.board[rowIndex][i]);
          }else{
            this.board[rowIndex][i] = currCol[j];
            this.updateNum(rowIndex, i);
          }
          rowIndex++;
        }
        for(let k = rowIndex; k < 4; k++){
          this.board[k][i] = 0;
          this.updateNum(k, i);
        }
      }
    },
    leftMove: function (){
      for(let i = 0; i < 4; i++){
        let currRow = this.getRow(i);
        let colIndex = 0;
        for(let j = 0; j < currRow.length; j++){
          if(j + 1 < currRow.length && currRow[j] === currRow[j + 1]){
            if(currRow[j] * 2 !== 2048){
              this.board[i][colIndex] = currRow[j] * 2;
              this.updateNum(i, colIndex);
              j++;
            }
            this.addScore(this.board[i][colIndex]);
          }else{
            this.board[i][colIndex] = currRow[j];
            this.updateNum(i, colIndex);
          }
          colIndex++;
        }
        for(let k = colIndex; k < 4; k++){
          this.board[i][k] = 0;
          this.updateNum(i, k);
        }
      }
    },
    downMove: function (){
      for(let i = 0; i < 4; i++){
        let currCol = this.getCol(i);
        let rowIndex = 3;
        for(let j = currCol.length - 1; j >= 0; j--){
          if(j - 1 >= 0 && currCol[j] === currCol[j - 1]){
            if(currCol[j] * 2 !== 2048){
              this.board[rowIndex][i] = currCol[j] * 2;
              this.updateNum(rowIndex, i);
              j--;
            }
            this.addScore(this.board[rowIndex][i]);
          }else{
            this.board[rowIndex][i] = currCol[j];
            this.updateNum(rowIndex, i);
          }
          rowIndex--;
        }
        for(let k = rowIndex; k >= 0; k--){
          this.board[k][i] = 0;
          this.updateNum(k, i);
        }
      }
    },
    rightMove: function() {
      for(let i = 0; i < 4; i++){
        let currRow = this.getRow(i);
        let colIndex = 3;
        for(let j = currRow.length - 1; j >= 0; j--){
          if(j - 1 >= 0 && currRow[j] === currRow[j - 1]){
            if(currRow[j] * 2 !== 2048){
              this.board[i][colIndex] = currRow[j] * 2;
              this.updateNum(i, colIndex);
              j--;
            }
            this.addScore(this.board[i][colIndex]);
          }else{
            this.board[i][colIndex] = currRow[j];
            this.updateNum(i, colIndex);
          }
          colIndex--;
        }
        for(let k = colIndex; k >= 0; k--){
          this.board[i][k] = 0;
          this.updateNum(i, k);
        }
      }
    },
    getCol: function (ind){
      let arr = [];
      for(let i = 0; i < 4; i++){
        if(this.board[i][ind] !== 0){
          arr.push(this.board[i][ind]);
        }
      }
      return arr;
    },
    getRow: function (ind){
      let arr = [];
      for(let i = 0; i < 4; i++){
        if(this.board[ind][i] !== 0){
          arr.push(this.board[ind][i]);
        }
      }
      return arr;
    },
    upMovable: function (){
      for(let j = 0; j < 4; j++){
        for(let i = 1; i < 4; i++){
          if(this.board[i][j] !== 0){
            if(this.board[i - 1][j] === 0 || this.board[i-1][j] === this.board[i][j]){
              return true;
            }
          }
        }
      }
      return false;
    },
    downMovable: function (){
      for(let j = 0; j < 4; j++){
        for(let i = 2; i >= 0; i--){
          if(this.board[i][j] !== 0){
            if(this.board[i + 1][j] === 0 || this.board[i + 1][j] === this.board[i][j]){
              return true;
            }
          }
        }
      }
      return false;
    },
    leftMovable: function (){
      for(let i = 0 ; i < 4 ; i++ ){
        for(let j = 1 ; j < 4 ; j++ ) {
          if(this.board[i][j] !== 0) {
            if (this.board[i][j - 1] === 0 || this.board[i][j - 1] === this.board[i][j]) {
              return true;
            }
          }
        }
      }
      return false;
    },
    rightMovable: function (){
      for(let i = 0 ; i < 4 ; i++) {
        for (let j = 2; j >= 0; j--) {
          if (this.board[i][j] !== 0) {
            if (this.board[i][j + 1] === 0 || this.board[i][j + 1] === this.board[i][j]) {
              return true;
            }
          }
        }
      }
      return false;
    },
    checkFail: function (){
      if(!(this.upMovable() || this.downMovable() || this.leftMovable() || this.rightMovable())){
        alert("你输了！当前得分" + this.score + "，请重新开局！");
      }
    },
    addController: function (){
      window.addEventListener("keydown", event => {
        if(event.key === 'w' || event.key === 'W' || event.key === "ArrowUp"){
          if(this.upMovable()){
            this.upMove();
            this.generateNum();
          }else{
            this.checkFail();
          }
        }
        if(event.key === 'a' || event.key === 'A' || event.key === "ArrowLeft"){
          if(this.leftMovable()){
            this.leftMove();
            this.generateNum();
          }else{
            this.checkFail();
          }
        }
        if(event.key === 's' || event.key === 'S' || event.key === "ArrowDown"){
          if(this.downMovable()){
            this.downMove();
            this.generateNum();
          }else{
            this.checkFail();
          }
        }
        if(event.key === 'd' || event.key === 'D' || event.key === "ArrowRight"){
          if(this.rightMovable()){
            this.rightMove();
            this.generateNum();
          }else{
            this.checkFail();
          }
        }
      }, false);
    }
  }
}
</script>

<style>
  @import "style/main.css";
</style>
