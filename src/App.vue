<template>
  <div id="app">
    <div class="wrapper clearfix">
      <players
        v-bind:isWinner="isWinner"
        v-bind:scorePlayer="scorePlayer"
        v-bind:activePlayer="activePlayer"
        v-bind:currentScore="currentScore"
      />
      <controls
        v-bind:isPlaying="isPlaying"
        v-on:handleNewGame="handleNewGame"
        v-on:handleRollDice="handleRollDice"
        v-on:handleHollScore="handleHollScore"
        v-bind:finalScore="finalScore"
        v-on:handleChangeFinalScore="handleChangeFinalScore"
      />
      <dices v-bind:dices="dices" />
      <popup-rule v-bind:isOpenPopup="isOpenPopup" v-on:handleConfirm="handleConfirm" />
    </div>
  </div>
</template>

<script>
import Players from "./components/Players";
import Controls from "./components/Controls";
import Dices from "./components/Dices";
import PopupRule from "./components/PopupRule";
export default {
  name: "app",
  data() {
    return {
      scorePlayer: [13, 30],
      currentScore: 0,
      activePlayer: 0,
      isPlaying: false,
      isOpenPopup: false,
      dices: [1, 1],
      finalScore: 10,
    };
  },
  components: {
    Players,
    Controls,
    Dices,
    PopupRule
  },
  computed: {
    isWinner() {
      let {scorePlayer, finalScore} = this;
      if (scorePlayer[0] >= finalScore || scorePlayer[1] >= finalScore){
        this.isPlaying = false;
        return true
      }
      return false
    },
  },
  methods: {
    handleNewGame() {
      this.isOpenPopup = true;
    },
    handleConfirm() {
      this.isPlaying = true;
      this.isOpenPopup = false;
      this.activePlayer = 0;
      this.scorePlayer = [0, 0];
      this.currentScore = 0;
      this.dices = [1, 1];
    },
    handleRollDice() {
      if (this.isPlaying) {
        var d1 = Math.floor(Math.random() * 6) + 1;
        var d2 = Math.floor(Math.random() * 6) + 1;
        this.dices = [d1, d2];
        if (d1 == 1 || d2 == 1) {
          setTimeout(() => {
            alert(
              `Người chơi Player ${this.activePlayer +
                1} đã quay trúng số 1. Rất Tiếc.`
            );
          }, 10);
          this.nextPlayer();
        } else {
          // cộng dồn điểm
          this.currentScore = this.currentScore + d1 + d2;
        }
      } else {
        alert("Vui lòng New Game!");
      }
    },
    nextPlayer() {
      this.activePlayer = this.activePlayer === 0 ? 1 : 0;
      this.currentScore = 0;
    },
    handleHollScore() {
      if (this.isPlaying) {
        let { scorePlayer, activePlayer, currentScore } = this;
        let scoreOld = scorePlayer[activePlayer];

        // cách 1
        // let cloneScorePlayer = [...scorePlayer];
        // cloneScorePlayer[activePlayer] = scoreOld + currentScore;
        // this.scorePlayer = cloneScorePlayer;

        // cách 2
        this.$set(this.scorePlayer, activePlayer, scoreOld + currentScore);

        if (!this.isWinner) {
          this.nextPlayer();
        }
      } else {
        alert("Vui lòng New Game!");
      }
    },
    handleChangeFinalScore(e){
      var x = parseInt(e.target.value);
      if (isNaN(x)) {
        this.finalScore = ''
      } else {
        this.finalScore = x
      }
      console.log();
    }
  }
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.clearfix::after {
  content: "";
  display: table;
  clear: both;
}

body {
  background-image: linear-gradient(
      rgba(62, 20, 20, 0.4),
      rgba(62, 20, 20, 0.4)
    ),
    url("/public/assets/back.jpg");
  background-size: cover;
  background-position: center;
  font-family: Lato;
  font-weight: 300;
  position: relative;
  height: 100vh;
  color: #555;
}

.wrapper {
  width: 1000px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: #fff;
  box-shadow: 0px 10px 50px rgba(0, 0, 0, 0.3);
  overflow: hidden;
}
</style>
