<template>
  <div id="app">
    <table>
      <tr>
        <td class="box" id="0"></td>
        <td class="box" id="1"></td>
        <td class="box" id="2"></td>
      </tr>
      <tr>
        <td class="box" id="3"></td>
        <td class="box" id="4"></td>
        <td class="box" id="5"></td>
      </tr>
      <tr>
        <td class="box" id="6"></td>
        <td class="box" id="7"></td>
        <td class="box" id="8"></td>
      </tr>
    </table>
    <div class="endgame">
      <div class="text"></div>
    </div>
    <button @click="startGame">Replay</button>
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'

export default {
  name: "App",
  components: {
    // HelloWorld
  },
  data() {
    return {
      humanPlayer: "X",
      computerPlayer: "O",
      orignalBoard: [],
      boxs: [],
      winCombination: [
        [0, 1, 2],
        [0, 3, 6],
        [0, 4, 8],
        [1, 4, 7],
        [2, 5, 8],
        [2, 4, 6],
        [3, 4, 5],
        [6, 7, 8],
      ],
    };
  },
  mounted() {
    this.boxs = document.querySelectorAll(".box");
    this.startGame();
  },
  methods: {
    startGame() {
      document.querySelector(".endgame").style.display = "none";
      this.orignalBoard = Array.from(Array(9).keys());
      for (let box of this.boxs) {
        box.innerText = "";
        box.style.removeProperty("background-color");
        box.addEventListener("click", this.turnClick, false);
      }
    },
    turnClick(box) {
      this.turn(box.target.id);
    },
    turn(boxId) {
      this.orignalBoard[boxId] = this.humanPlayer;
      document.getElementById(boxId).innerText = this.humanPlayer;
      let gameWon = this.checkWin();
      if(gameWon) {
        this.gameOver(gameWon);
      }
    },

    checkWin() {
      let plays = this.orignalBoard.reduce((acc, e, i) => (e === this.humanPlayer) ? [...acc,i] : acc, []);
      let gameWon = null;
      for(let [index, element] of this.winCombination.entries()) {
        if (element.every(e => plays.indexOf(e) > -1)) {
          gameWon = {index: index, player: this.humanPlayer};
          break;
        }
      }
      return gameWon;
    },

    gameOver(gameWon) {
      for (let index of this.winCombination[gameWon.index]) {
        document.getElementById(index).style.backgroundColor = 
        gameWon.player == this.humanPlayer ? "lightBlue" : "red";
      }
      for (let box of this.boxs) {
        box.removeEventListener('click', this.turnClick, false);
      }
    },
  },
};
</script>

<style scoped>
/* #app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
} */

.header {
  width: 50%;
  display: flex;
  justify-content: space-between;
}

td {
  border: 2px solid #333;
  height: 100px;
  width: 100px;
  text-align: center;
  vertical-align: middle;
  font-family: "Comic Sans MS", cursive, sans-serif;
  font-size: 70px;
  cursor: pointer;
}

table {
  border-collapse: collapse;
  position: absolute;
  left: 50%;
  margin-left: -155px;
  top: 50px;
}

table tr:first-child td {
  border-top: 0;
}

table tr:last-child td {
  border-bottom: 0;
}

table tr td:last-child {
  border-right: 0;
}

table tr td:first-child {
  border-left: 0;
}

.endgame {
  display: none;
  width: 200px;
  top: 120px;
  background-color: rgba((205, 133, 63, 0.8), green, blue, alpha);
  position: absolute;
  left: 50%;
  margin-left: -100px;
  padding-top: 50px;
  padding-bottom: 50px;
  text-align: center;
  border-radius: 5px;
  color: white;
  font-size: 2em;
}
</style>
