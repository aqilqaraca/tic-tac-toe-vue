<template>
    <div class="block">
    <div class="game-board">
      <h4 v-if="winner" class="winner text-center">Game Over! , Winner is {{ winner }}</h4>
      <hr v-if="winner">
    <table cellspacing="0">
      <tr v-for="(row,rowIndex) in game" :key="rowIndex">
        <td v-for="(column,columnIndex) in row" :key="columnIndex" @click="click(rowIndex,columnIndex)">
          <transition name="fade">
          <span v-if="game[rowIndex][columnIndex]">{{ game[rowIndex][columnIndex] }}</span>
          </transition>
        </td>
      </tr>
      
    </table>
    <button v-if="rstbtn" class="btn btn-light reset text-uppercase" @click="reset()">Restart Game</button>

  </div>
  
  </div>
</template>

<script>
export default {
  data(){
    return{
      turn : "X",
      winner : null,
      rstbtn : false,
      game : [
        ["","",""],
        ["","",""],
        ["","",""]
      ]
    }
  },
  methods : {
    moveComputer(){
      while(true){
        const randomX = Math.floor(Math.random()*3)
        const randomY = Math.floor(Math.random()*3)
        if(this.game[randomX][randomY] === "")
        {
          this.game[randomX][randomY] = "O"
          break;
        }
        
      }
      
    },
    click(x,y){
      if(this.winner || this.game[x][y] || this.turn ==="O"){
        return
      }

      this.turn = "O"
      this.game[x][y] = "X"

      let emptyCount = 0;
      for(let i = 0; i<3; i++){
        for(let k = 0; k<3; k++){
          if(this.game[i][k] === ""){
            emptyCount++
          }
        }
      }

      if(this.isDone()){
        this.winner = "X"
      }
      else if(emptyCount===0){
        this.winner = "X and O"
      }else{
        setTimeout(()=>{
        this.moveComputer()
        if(this.isDone()){
          this.winner = "O"
        }
        this.turn = "X"
        this.$forceUpdate()
        },500)
      }
      this.rstbtn = true;
      this.$forceUpdate()
    },
    isDone(){
      for(let i = 0; i<3; i++){
        if(this.game[i][0] && this.game[i][0] ===this.game[i][1] && this.game[i][0]===this.game[i][2]){
          return true
        }
      }
      for(let i = 0; i<3; i++){
        if(this.game[0][i] && this.game[0][i] === this.game[1][i] && this.game[0][i]==this.game[2][i]){
          return true
        }
      }
      if(this.game[0][0] && this.game[0][0] === this.game[1][1] && this.game[0][0]==this.game[2][2]){
        return true
      }
      if(this.game[0][2] && this.game[0][2] === this.game[1][1] && this.game[0][2]==this.game[2][0]){
        return true
      }
      
      return false
    },
    reset(){
      this.rstbtn = false
      this.winner = null
      this.game = [["","",""],["","",""],["","",""]]
    },
    
  }
}
</script>

<style>
.winner{
  top: 0;
  background-color: #fff;
  color: #14bdac;
  width: 100%;
  position: absolute;
  height: 70px;
  line-height: 70px;
  border-bottom-right-radius: 20px;
  border-bottom-left-radius: 20px;
}
.block{
  position: relative;
}
.reset{
  width: 100%;
  position: absolute;
  height: 70px;
  font-size: 26px;
  font-weight: 500;
  color: #14bdac;
  bottom: 0;
  border-top-right-radius: 20px;
  border-top-left-radius: 20px;
}
.reset:hover{
  transition: all ease 1s;
}
.game-board {
  background-color: #14bdac;
  height: 100vh;
  display: flex;
}
.game-board table{
  margin: auto;
}

.game-board td{
  height: 80px;
  width: 80px;
  text-align: center;
  font-size: 60px;
  font-weight: 500;
  color: #fff;
  line-height: 80px;

}

.game-board table tr td:nth-child(1),
.game-board table tr td:nth-child(2){
  border-right: 5px solid #0da192;
}

.game-board table tr:nth-child(1) td,
.game-board table tr:nth-child(2) td{
  border-bottom: 5px solid #0da192;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to {
  opacity: 0;
}


</style>