<template>
  <h1 class="title">Reaction timer</h1>
  <h2 class="ins">Click the square</h2>
  <div class="game-frame">
    <h1 class="levelB">{{ level }}</h1>
    <Game v-if="isPlaying" :delay="delay" @gameBreak="breakGame" @levelUpdate = "levelUpdate"/>
  </div>
  <Break v-if="gameBreak">
    <h1>Oops! Too Slow</h1>
    <h2 class="output-style">Level: {{ level }}/4</h2>
    <h2 class="output-style">Streak: {{ streak }}/5</h2>
    <button @click="restartGame">Restart</button>
  </Break>
  <Break v-if="gameFinish">
    <h1>Well done, You are fast as a cat</h1>
    <button @click="restartGame">Restart</button>
  </Break>
  <button @click="startGame" v-if="!gameBreak" :disabled = "buttonDisabled">Start Game</button>
</template>

<script>
import Game from './components/Game.vue'
import Break from './components/GameBreak.vue'

export default {
  name: 'App',
  components: {
    Game, Break
  },
  data() {
    return {
      isPlaying: false,
      delay: null,
      gameBreak: false,
      gameFinish: false,
      strak: 0,
      level: 1,
      buttonDisabled: false
    }
  },
  methods:{
    startGame() {
      this.delay = 500 + Math.random() * 2000
      this.isPlaying = true
      this.buttonDisabled = true
    },
    breakGame(gameBreakArray) {
      this.isPlaying = false
      this.gameBreak = gameBreakArray[0]
      this.streak = gameBreakArray[1]
      this.level = gameBreakArray[2]
      this.gameFinish = gameBreakArray[3]
    },
    restartGame() {
      this.level = 1
      this.gameBreak = false
      this.gameFinish = false
      this.startGame()
    },
    levelUpdate(level) {
      this.level = level
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin-top: 60px;
}
body{
  background-color: rgba(0, 0, 0, 0.8);
}
.game-frame{
    width: 700px;
    height: 400px;
    border: 4px solid lightgray;
    margin: 20px auto;
    position: relative;
    background-color: white;
    text-align: center;
    display: flex;
    align-items: center;
    justify-content: center;
}
.title{
  font-family: cursive;
  color: white;
  font-size: 30px;
  margin: 0;
}
.ins{
  padding: 0;
  margin: 0;
  font-family: cursive;
  color: white;
  font-size: 15px;
}
button{
  font-size: 25px;
  padding: 5px 10px;
  border: none;
  cursor: pointer;
  outline: none;
  font-family: Georgia, 'Times New Roman', Times, serif;
  
}
button:active{
  transform: scale(0.97);
}
.output-style{
  font-family: monospace;
}
.levelB{
  color: rgba(0, 0, 0, 0.1);
  font-size: 250px;
}
</style>
