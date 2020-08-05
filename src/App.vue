<template>
  <div>
    <template>
      <div class="wrapper">
        <h1>Simon Says</h1>
        <div class="game-board">
          <div class="simon">
            <ul>
              <li
                @click="checkUserSiquence('red')"
                v-bind:class="{active: isActive == 'red'}"
                class="red tile"
              ></li>
              <li
                @click="checkUserSiquence('yellow')"
                v-bind:class="{active: isActive == 'yellow'}"
                class="tile yellow"
              ></li>
              <li
                @click="checkUserSiquence('blue')"
                v-bind:class="{active: isActive == 'blue'}"
                class="blue tile"
              ></li>
              <li
                @click="checkUserSiquence('green')"
                v-bind:class="{active: isActive == 'green'}"
                class="green tile"
              ></li>
            </ul>
          </div>
        </div>
        <div class="game-info">
          <h2>
            Round:
            <span data-round="0">{{round}}</span>
          </h2>
          <button @click="startNewGame" data-action="start">Start</button>
          <p v-show="gameOver" data-action="lose">
            Sorry, you lost after
            <span data-round="0">{{round}}</span> rounds!
          </p>
        </div>
        <div class="game-options">
          <h2>Game Options:</h2>
          <input
            v-model="gameOption"
            id="Easy"
            type="radio"
            name="mode"
            value="1500"
            v-model:="Changed"
          />Easy
          <br />
          <input v-model="gameOption" id="Middle" type="radio" name="mode" value="1000" />Middle
          <br />
          <input v-model="gameOption" id="Hard" type="radio" name="mode" value="400" />Hard
          <br />
        </div>
        <div data-action="sound"></div>
      </div>
    </template>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      tile: ["red", "yellow", "blue", "green"],
      audios: {
        red: new Audio(require("./assets/sounds/1.mp3")),
        yellow: new Audio(require("./assets/sounds/2.mp3")),
        blue: new Audio(require("./assets/sounds/3.mp3")),
        green: new Audio(require("./assets/sounds/4.mp3")),
      },
      isActive: null, //manages the classes that define the active color
      gameOption: 1500,
      sequenceSimon: [],
      sequenceColors: [],
      round: 0,
      i: 0, // i it's iterating over an array of color sequence sequenceSimon[]

      userCounter: 0,
      gameOver: false,
    };
  },

  methods: {
    startNewGame() {
      this.isActive = null;
      this.sequenceSimon.length = 0;
      this.sequenceSimon.length = 0;
      this.i = 0;
      this.userCounter = 0;
      this.round = 0;
      this.gameOver = false;
      this.newStep();
    },

    newStep() {
      this.sequenceSimon.push(
        this.tile[Math.floor(Math.random() * this.tile.length)]
      );
      console.log(this.gameOption);
      this.isActive = this.sequenceSimon[this.i];
      this.round++;
      this.i = 0;
      this.userCounter = 0;
      this.animationSimon();
    },
    //animation of sequence
    animationSimon() {
      if (this.i < this.sequenceSimon.length) {
        if (this.sequenceSimon[this.i] == this.sequenceSimon[this.i - 1]) {
          setTimeout(() => {
            this.isActive = this.sequenceSimon[this.i];
            this.playAudio(this.isActive);
          }, 500);
        } else {
          this.isActive = this.sequenceSimon[this.i];
          this.playAudio(this.isActive);
        }
        setTimeout(() => {
          this.isActive = null;
          this.i++;
          this.animationSimon();
        }, this.gameOption);
      }
    },

    checkUserSiquence(color) {
      if (this.sequenceSimon != []) {
        if (color == this.sequenceSimon[this.userCounter]) {
          this.playerClickAnimation(color);
          this.userCounter++;
          if (this.userCounter === this.sequenceSimon.length) {
            setTimeout(() => {
              this.newStep();
            }, 1000);
          }
        } else {
          this.playerClickAnimation(color);
          this.gameOver = true;
          this.sequenceSimon.length = 0;
          this.sequenceSimon.length = 0;
          this.i = 0;
          this.userCounter = 0;
          this.isActive = null;
        }
      }
    },

    playerClickAnimation(activeColor) {
      this.isActive = activeColor;
      this.playAudio(this.isActive);

      setTimeout(() => {
        this.isActive = null;
      }, 400);
    },

    playAudio(soundByColor) {
      this.audios[soundByColor].play();
    },
  },
};
</script>

<style>
</style>
