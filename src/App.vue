<template>
  <div id="app">
    <div class="header">
      <h1>Player: {{ player }}</h1>
      <button @click.prevent="reset_game">Reset</button>
    </div>
    <div id="battle_field">
      <div v-for="row in 3" :key="row" class="line">
        <button
          v-for="col in 3"
          :key="col"
          :id="'box' + row + col"
          class="box"
          @click.prevent="play"
        ></button>
      </div>
    </div>
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
      player: "X",
      mark: "",
    };
  },
  methods: {
    play(e) {
      e.target.innerText = this.player;
      // e.target.outerText = this.player;
      e.target.disabled = true;
      this.player === "X" ? (this.player = "O") : (this.player = "X");
    },
    reset_game() {
      const boxs = document.getElementsByClassName("box");
      console.log(boxs);
      for(let box of boxs) {
        box.innerText = '';
        box.disabled = false;
      }
    }
  },
};
</script>

<style scoped>
#app {
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
}

.header {
  width: 50%;
  display: flex;
  justify-content: space-between;
}
#battle_field {
  background-color: lightblue;
  width: 50%;
  height: 100%;
  display: flex;
  flex-direction: column;
}

.box {
  color: white;
  font-size: 5rem;
  width: 100%;
  height: 100%;
  border: 1px solid red;
}

.line {
  display: flex;
  flex-wrap: nowrap;
  width: 100%;
  height: 100%;
}
</style>
