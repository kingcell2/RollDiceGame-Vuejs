<template>
  <div id="App">
    <div class="wrapper clearfix">
      <players
        v-bind:isWinner="isWinner"
        v-bind:activePlayer="activePlayer"
        v-bind:scoresPlayer="scoresPlayer"
        v-bind:currentScore="currentScore"
      />
      <controls
        v-bind:bet="bet"
        v-bind:isPlaying="isPlaying"
        v-bind:finalScore="finalScore"
        v-on:handleChangeFinalScore="handleChangeFinalScore"
        v-on:handleHollScore="handleHollScore"
        v-on:handleNewGame="handleNewGame"
        v-on:handleRollDice="handleRollDice"
        v-on:handleRollSpecial="handleRollSpecial"
        v-on:handleRollBet="handleRollBet"
      />
      <dices v-bind:dices="dices" />
      <popup-rule
        v-bind:finalScore="finalScore"
        v-on:handleConfirm="handleConfirm"
        v-on:handleExit="handleExit"
        v-on:handleYes="handleYes"
        v-on:handleNo="handleNo"
        v-bind:isOpenPopup="isOpenPopup"
        v-bind:isOpenPopup_choice="isOpenPopup_choice"
      />
    </div>
  </div>
</template>

<script>
import Controls from "./components/Controls.vue";
import Dices from "./components/Dices.vue";
import Players from "./components/Players.vue";
import PopupRule from "./components/PopupRule.vue";
export default {
  name: "App",
  data() {
    return {
      isOpenPopup: false,
      isOpenPopup_choice: false,
      isPlaying: false,
      activePlayer: 0,
      scoresPlayer: [14, 30],
      dices: [3, 6],
      currentScore: 30,
      finalScore: 60,
      bet: false,
    };
  },

  methods: {
    handleYes() {
      if (this.isPlaying) {
        let { scoresPlayer, activePlayer } = this;
        let scoreOld = scoresPlayer[activePlayer];
        if (scoreOld < this.finalScore / 3) {
          alert("Bạn không đủ điều kiện để đánh cược");
          this.handleNo();
        } else {
          this.bet = true;
          // alert("Đánh cược với {{ finalScore / 3 }} hiện có");
          this.handleNo();
          scoreOld = scoreOld / 3;
          this.$set(this.scoresPlayer, activePlayer, scoreOld);
        }
      }
      // this.isOpenPopup_choice = true;
      //Đánh cược điểm
    },
    handleChangeFinalScore(e) {
      var number = parseInt(e.target.value);
      if (isNaN(number)) {
        this.finalScore = "";
      } else {
        this.finalScore = number;
      }
    },
    handleHollScore() {
      if (this.isPlaying) {
        let { scoresPlayer, activePlayer, currentScore } = this;
        let scoreOld = scoresPlayer[activePlayer];
        this.$set(this.scoresPlayer, activePlayer, scoreOld + currentScore);

        if (!this.isWinner) {
          this.nextPlayers();
        }
      } else {
        alert("Vui lòng nhấn vào nút NewGame");
      }
    },
    nextPlayers() {
      this.activePlayer = this.activePlayer === 0 ? 1 : 0;
      this.currentScore = 0;
      this.bet = false;
    },
    handleConfirm() {
      this.isPlaying = true;
      this.isOpenPopup = false;
      this.isOpenPopup_choice = false;
      this.activePlayer = 0;
      this.dices = [1, 1];
      this.scoresPlayer = [0, 0];
      this.currentScore = 0;
    },

    handleNewGame() {
      //Hien thi popup -> show luat choi
      this.isOpenPopup = true;
      // this.isOpenPopup_choice = true;
    },

    handleExit() {
      //Hien thi popup -> dong luat choi
      this.isOpenPopup = false;
    },
    handleRollBet() {
      if (this.bet && this.isPlaying) {
        var dice1 = Math.floor(Math.random() * 6) + 1;
        var dice2 = Math.floor(Math.random() * 6) + 1;
        this.dices = [dice1, dice2];
        if (dice1 == 1 || dice2 == 1) {
          //đổi lượt chơi
          let activePlayer = this.activePlayer;
          setTimeout(function () {
            alert(
              `Người chơi Player ${
                activePlayer + 1
              } đã quay trúng ô số 1, rất tiếc! `
            );
          }, 20);

          this.nextPlayers();
        } else {
          this.currentScore = this.currentScore + (dice1 + dice2) * 3;
        }
      } else {
        alert("Vui lòng ấn vào nút Bet");
      }
    },
    handleRollSpecial() {
      if (this.isPlaying) {
        this.isOpenPopup_choice = true;
      } else {
        alert("Vui lòng ấn vào nút Bet để đánh cược");
      }
    },
    handleNo() {
      this.isOpenPopup_choice = false;
    },

    handleRollDice() {
      // console.log("rolldice app.vue");
      if (this.isPlaying) {
        //Xoay xúc xắc
        var dice1 = Math.floor(Math.random() * 6) + 1;
        var dice2 = Math.floor(Math.random() * 6) + 1;
        this.dices = [dice1, dice2];
        if (dice1 == 1 || dice2 == 1) {
          //đổi lượt chơi
          let activePlayer = this.activePlayer;
          setTimeout(function () {
            alert(
              `Người chơi Player ${
                activePlayer + 1
              } đã quay trúng ô số 1, rất tiếc! `
            );
          }, 20);

          this.nextPlayers();
        } else {
          this.currentScore = this.currentScore + dice1 + dice2;
        }
        // console.log(dice1, dice2);
      } else {
        alert("Vui lòng nhấn vào nút NewGame");
      }
    },
  },
  components: {
    Players,
    Controls,
    Dices,
    PopupRule,
  },
  computed: {
    isWinner() {
      let { scoresPlayer, finalScore } = this;
      if (scoresPlayer[0] >= finalScore || scoresPlayer[1] >= finalScore) {
        // this.isPlaying == false;
        return true;
      }
      return false;
    },
  },
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
    url("../public/assets/back.jpg");
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
