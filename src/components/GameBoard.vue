<template>

  <div ref="win-banner" v-if="win" class="alert alert-success alert-dismissible" role="alert">
    <h4 class="alert-heading">Well done!</h4>
    <p>Aww yeah, you successfully finished this game in {{ this.actualRow }} rounds</p>
  </div>

  <div class="main-container">
    <div class="heading-wrapper">
      <button @click="initGame" type="button" class="btn btn-primary">Reset Game</button>
    </div>

    <div class="board-wrapper">
      <div v-for="(playerGuessRow, index) in playerGuess" :key="playerGuessRow">
        <div class="row-wrapper">
          <div class="row-guess-wrapper">
            <span class="guess-round" :class="playerGuess[index][0]"></span>
            <span class="guess-round" :class="playerGuess[index][1]"></span>
            <span class="guess-round" :class="playerGuess[index][2]"></span>
            <span class="guess-round" :class="playerGuess[index][3]"></span>
          </div>
          <div class="row-result-wrapper">
            <span><resolved-bulb :color="resolvedRow[index][0]"/></span>
            <span><resolved-bulb :color="resolvedRow[index][1]"/></span>
            <span><resolved-bulb :color="resolvedRow[index][2]"/></span>
            <span><resolved-bulb :color="resolvedRow[index][3]"/></span>
          </div>
        </div>
        <hr/>
      </div>
    </div>

    <div class="select-buttons-wrapper">
      <player-guess-button v-for="color in allColors" @click="addPlayerGuess(color)" :color="color"/>
      <button :disabled="isRoundInProgress" @click="resolveRow" type="button" class="btn btn-primary btn-sm">Resolve
        row
      </button>
    </div>
  </div>

  <nav class="bottom-for-small-devices navbar fixed-bottom navbar-light bg-light navbar-custom">

    <div class="navbar-buttons-wrapper">
      <div class="navbar-select-buttons-wrapper">
        <button v-for="color in allColors" type="button" @click="addPlayerGuess(color)" class="btn btn-circle btn-xs"
                :class="color"/>
      </div>
    <button :disabled="isRoundInProgress" @click="resolveRow" type="button" class="btn btn-primary btn-sm">Resolve Round</button>
    </div>
  </nav>


</template>

<script>
import ResolvedBulb from "./ResolvedBulb.vue";
import PlayerGuessButton from "./PlayerGuessButton.vue";

const boardSize = 10;

export default {
  name: "GameBoard",
  components: {PlayerGuessButton, ResolvedBulb},
  created() {
    this.initGame()
  },

  data() {
    return {
      win: false,
      isRoundInProgress: true,
      allColors: ['yellow', 'violet', 'blue', 'green', 'red', 'orange'],
      secret: [],
      actualRow: 0,
      playerGuess: new Array(boardSize).fill(0).map(() => new Array(4).fill(0)),
      resolvedRow: new Array(boardSize).fill(0).map(() => new Array(4).fill(0)),
    }

  },
  methods: {

    initGame() {
      this.win = false;
      this.actualRow = 0;
      this.playerGuess = new Array(boardSize).fill(0).map(() => new Array(4).fill(0));
      this.resolvedRow = new Array(boardSize).fill(0).map(() => new Array(4).fill(0));
      this.createRandomColorsSecret();
    },

    addPlayerGuess(color) {
      if (this.playerGuess[this.actualRow] && this.playerGuess[this.actualRow].length === 4) {
        this.playerGuess[this.actualRow] = [];
      }
      this.playerGuess[this.actualRow].push(color);

      this.isRoundInProgress = this.playerGuess[this.actualRow].length !== 4;
    },

    resolveRow() {
      this.resolvedRow[this.actualRow] = []
      for (let i = 0; i < 4; i++) {
        if (this.secret[i] === this.playerGuess[this.actualRow][i]) {
          this.resolvedRow[this.actualRow].push('black')
        } else if (this.playerGuess[this.actualRow].some(guess => guess === this.secret[i])) {
          this.resolvedRow[this.actualRow].push('white')
        }
      }

      this.win = this.resolvedRow[this.actualRow].every(r => r === 'black');

      if (this.win === true) {
        this.$nextTick(() => {
          this.$refs["win-banner"].scrollTop = 0;
        });
      }


      this.actualRow++
      this.isRoundInProgress = true;

    },


    pickRandomColor() {
      return this.allColors[Math.floor(Math.random() * 6)];
    },


    createRandomColorsSecret() {
      let randomSecret = [];

      let index = 0;
      do {

        let randomColor = this.pickRandomColor();

        if (!randomSecret.some(c => c === randomColor)) {
          randomSecret.push(randomColor)
        }

        index++
      } while (randomSecret.length < 4)

      this.secret = randomSecret;

      console.log(`Super secret colors are: ${this.secret}`)
    },


  }

}
</script>

<style scoped>

.heading-wrapper {
  margin: 3em;
}

.select-buttons-wrapper {
  margin: 3em auto;
  width: 50vw;
}

.navbar-buttons-wrapper {
  margin: auto;
  display: flex;
  align-items: center;
  flex-direction: column;
}

.navbar-custom {
  height: 100px;
}
.navbar-select-buttons-wrapper {
  margin-bottom: 4px;
}

.row-wrapper {
  display: flex;
}

.guess-round {
  background-color: cornsilk;
  margin: 5px;
  height: 3em;
  width: 3em;
  border: solid grey;
  border-radius: 50%;
  display: inline-block;

}


@media (max-width: 1080px) {
  .guess-round {
    height: 2em;
    width: 2em;
  }

}

hr {
  margin: 5px;
  height: 2px;

}

.row-guess-wrapper {
  display: flex;
  justify-content: space-around;
  width: 70vw;
  margin: 0.5em;
}

.row-result-wrapper {
  display: flex;
  justify-content: space-around;
  width: 30vw;
  margin: 0.5em;
}

@media (max-width: 1080px) {
  .row-result-wrapper {
    flex-wrap: wrap;
  }

}


.board-wrapper {
  display: flex;
  flex-direction: column;
  margin: 3vw auto;
  background-color: cornsilk;
  border-style: solid;
  border-radius: 1em;
}

@media (min-width: 450px) {
  .bottom-for-small-devices {
    display: none;
  }
}

@media (max-width: 450px) {
  .select-buttons-wrapper {
    display: none;
  }

}

@media (min-width: 1080px) {

  .board-wrapper {
    max-width: 50%;

  }
}

@media (max-width: 1080px) {
  .board-wrapper {
    max-width: 100%;
  }
}


.btn-circle.btn-xs {
  margin: 5px;
  width: 40px;
  height: 40px;
  padding: 6px 0px;
  border-radius: 20px;
  font-size: 8px;
  text-align: center;
}

.red {
  background-color: crimson;
}

.blue {
  background-color: aqua;
}

.orange {
  background-color: orange;
}

.green {
  background-color: greenyellow;
}

.violet {
  background-color: mediumvioletred;
}

.yellow {
  background-color: yellow;
}
</style>