<template>
  <div class="simon-game">
    <div>
      <SimonButton
        @click="onColorClick('green')"
        :class="this.isActive.green && 'active'"
        :color="'green'"
      />
      <SimonButton
        @click="onColorClick('red')"
        :class="this.isActive.red && 'active'"
        :color="'red'"
      />
    </div>
    <div>
      <SimonButton
        @click="onColorClick('yellow')"
        :class="this.isActive.yellow && 'active'"
        :color="'yellow'"
      />
      <SimonButton
        @click="onColorClick('blue')"
        :class="this.isActive.blue && 'active'"
        :color="'blue'"
      />
    </div>
    <button @click="startGame" class="start-button">
      {{ mainButtonText }}
    </button>
    <h3 class="info-text">Round: {{ currentRound }}</h3>
    <h3 class="info-text">
      {{ isGameStarted ? (isUserMove ? "Your turn" : "Game turn") : "" }}
    </h3>
    <div>
      <button
        :class="
          levelComplexity === 1500 ? 'active-complexity' : 'complexity-btn'
        "
        @click="this.levelComplexity = 1500"
      >
        easy
      </button>
      <button
        :class="
          levelComplexity === 1000 ? 'active-complexity' : 'complexity-btn'
        "
        @click="this.levelComplexity = 1000"
      >
        normal
      </button>
      <button
        :class="
          levelComplexity === 400 ? 'active-complexity' : 'complexity-btn'
        "
        @click="this.levelComplexity = 400"
      >
        hard
      </button>
    </div>
  </div>
</template>

<script>
import SimonButton from "./SimonButton.vue";
export default {
  name: "simon-game",
  components: {
    SimonButton,
  },
  data() {
    return {
      colors: ["green", "red", "yellow", "blue"],
      isGameStarted: false,
      isUserMove: false,
      gameSteps: [],
      userSteps: [],
      currentRound: 0,
      stepInterval: null,
      levelComplexity: 1000,
      isActive: {
        green: false,
        red: false,
        yellow: false,
        blue: false,
      },
      mainButtonText: "Start the game",
    };
  },
  methods: {
    checkStep() {
      for (let i = 0; i < this.userSteps.length; i++) {
        if (this.userSteps[i] !== this.gameSteps[i]) {
          this.mainButtonText = "wrong";
          this.gameSteps = [];
          this.userSteps = [];
          this.isUserMove = false;
          this.isGameStarted = false;
          setTimeout(() => {
            this.mainButtonText = "Restart the game";
          }, 1000);
        }
      }
    },
    onColorClick(color) {
      if (this.isUserMove) {
        this.userSteps.push(color);
        this.checkStep();
      }
      if (this.isUserMove && this.userSteps.length === this.gameSteps.length) {
        this.userSteps = [];
        this.gameMove();
      }
    },
    colorToggle(color) {
      this.isActive[color] = !this.isActive[color];
    },
    gameMove() {
      this.isUserMove = false;
      const color = this.colors[Math.floor(Math.random() * 4)];
      this.gameSteps.push(color);
      this.currentRound = this.gameSteps.length;
      let index = 0;
      this.stepInterval = setInterval(() => {
        this.colorToggle(this.gameSteps[index]);
        if (index >= this.gameSteps.length) {
          clearInterval(this.stepInterval);
          this.isUserMove = true;
        }
        setTimeout(() => {
          this.colorToggle(this.gameSteps[index]);
          index += 1;
        }, this.levelComplexity / 2);
      }, this.levelComplexity);
    },
    startGame() {
      clearInterval(this.stepInterval);
      this.isUserMove = false;
      this.isGameStarted = true;
      this.userSteps = [];
      this.gameSteps = [];
      this.mainButtonText = "Restart the game";
      this.isActive = { blue: false, green: false, yellow: false, red: false };
      this.gameMove();
    },
  },
};
</script>

<style scoped>
.simon-game {
  text-align: center;
  margin-top: 20vh;
}
.start-button {
  display: inline-block;
  position: absolute;
  top: 30vh;
  left: calc(50vw - 10vh);
  width: 20vh;
  height: 20vh;
  border-radius: 50%;
  border: 2px solid black;
  background-color: black;
  color: #ffffff;
  font-size: 3vh;
}
.start-button:active {
  background-color: gray;
}
.info-text {
  color: white;
  font-size: 30px;
}
.complexity-btn {
  font-size: 25px;
  padding: 10px 25px;
}
.active-complexity {
  font-size: 25px;
  padding: 10px 25px;
  background-color: aqua;
}
</style>
