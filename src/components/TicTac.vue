<script setup lang="ts">
import { ref } from "vue";
import { GameSquare } from "../models/GameSquare";
import { Player } from "../models/Player";

let gameBoard = ref<GameSquare[]>([
  { checkedO: false, checkedX: false },
  { checkedO: false, checkedX: false },
  { checkedO: false, checkedX: false },
  { checkedO: false, checkedX: false },
  { checkedO: false, checkedX: false },
  { checkedO: false, checkedX: false },
  { checkedO: false, checkedX: false },
  { checkedO: false, checkedX: false },
  { checkedO: false, checkedX: false },
]);

let showStartButton = ref(true);

const firstMove = () => {
  let startNumber = Math.floor(Math.random() * (3 - 1) + 1);
  console.log("nummer", startNumber);
  if (startNumber < 2) {
    props.playerO.turn = true;
  } else {
    props.playerX.turn = true;
  }
  showStartButton.value = false;
};

const toggleTurn = () => {
  props.playerO.turn = !props.playerO.turn;
  props.playerX.turn = !props.playerX.turn;
};

let playable = true;
let test = ref("test");

const handleClick = (index: number) => {
  if (playable) {
    console.log("playable", playable);
    markSquare(index);
  }
  if (!playable) {
    console.log(test);
  }
};

const markSquare = (index: number) => {
  if (props.playerO.turn) {
    gameBoard.value[index].checkedO = !gameBoard.value[index].checkedO;
  } else {
    gameBoard.value[index].checkedX = !gameBoard.value[index].checkedX;
  }
  toggleTurn();
  checkWinner();
};

const newGame = () => {
  for (let i = 0; i < gameBoard.value.length; i++) {
    gameBoard.value[i].checkedO = false;
    gameBoard.value[i].checkedX = false;
  }

  showStartButton.value = true;
  props.playerO.turn = false;
  props.playerX.turn = false;
  showWinner.value = false;
  playable = true;
};

/*******************************************************************/

const checkWinner = () => {
  //vågrät
  if (
    (gameBoard.value[0].checkedX &&
      gameBoard.value[1].checkedX &&
      gameBoard.value[2].checkedX) ||
    (gameBoard.value[0].checkedO &&
      gameBoard.value[1].checkedO &&
      gameBoard.value[2].checkedO)
  ) {
    winner();
  }
  if (
    (gameBoard.value[3].checkedX &&
      gameBoard.value[4].checkedX &&
      gameBoard.value[5].checkedX) ||
    (gameBoard.value[3].checkedO &&
      gameBoard.value[4].checkedO &&
      gameBoard.value[5].checkedO)
  ) {
    winner();
  }
  if (
    (gameBoard.value[6].checkedX &&
      gameBoard.value[7].checkedX &&
      gameBoard.value[8].checkedX) ||
    (gameBoard.value[6].checkedO &&
      gameBoard.value[7].checkedO &&
      gameBoard.value[8].checkedO)
  ) {
    winner();
  }

  //lodrät
  if (
    (gameBoard.value[0].checkedO &&
      gameBoard.value[3].checkedO &&
      gameBoard.value[6].checkedO) ||
    (gameBoard.value[0].checkedX &&
      gameBoard.value[3].checkedX &&
      gameBoard.value[6].checkedX)
  ) {
    winner();
  }
  if (
    (gameBoard.value[1].checkedO &&
      gameBoard.value[4].checkedO &&
      gameBoard.value[7].checkedO) ||
    (gameBoard.value[1].checkedX &&
      gameBoard.value[4].checkedX &&
      gameBoard.value[7].checkedX)
  ) {
    winner();
  }
  if (
    (gameBoard.value[2].checkedO &&
      gameBoard.value[5].checkedO &&
      gameBoard.value[8].checkedO) ||
    (gameBoard.value[2].checkedX &&
      gameBoard.value[5].checkedX &&
      gameBoard.value[8].checkedX)
  ) {
    winner();
  }
  //diagonal
  if (
    (gameBoard.value[6].checkedO &&
      gameBoard.value[4].checkedO &&
      gameBoard.value[2].checkedO) ||
    (gameBoard.value[6].checkedX &&
      gameBoard.value[4].checkedX &&
      gameBoard.value[2].checkedX)
  ) {
    winner();
  }
  if (
    (gameBoard.value[8].checkedO &&
      gameBoard.value[4].checkedO &&
      gameBoard.value[0].checkedO) ||
    (gameBoard.value[8].checkedX &&
      gameBoard.value[4].checkedX &&
      gameBoard.value[0].checkedX)
  ) {
    winner();
  }
};

let showWinner = ref(false);

const winner = () => {
  showWinner.value = true;
  playable = false;
};

/*******************************************************************/

interface ITicTacProps {
  playerX: Player;
  playerO: Player;
}

const props = defineProps<ITicTacProps>();

defineEmits(["toggleSquare"]);
const showPlayerO = ref({ playing: props.playerO.turn });
const showPlayerX = ref({ playing: props.playerX.turn });
</script>
<template>
  <div class="gamePage">
    <div class="infoPanel">
      <p class="players" :class="showPlayerX">
        {{ playerX.playerName }} spelar som X
      </p>
      <p class="players" :class="showPlayerO">
        {{ playerO.playerName }} spelar som O
      </p>

      <div class="winner" v-if="showWinner">
        <p v-if="playerO.turn">{{ playerX.playerName }} vann!</p>
        <p v-else>{{ playerO.playerName }} vann!</p>
      </div>

      <button v-if="showStartButton" @click="firstMove">Starta spelet</button>
      <button v-else @click="newGame">Börja om</button>
    </div>

    <div v-if="!showStartButton" class="gameContainer">
      <div
        class="gameSquare"
        v-for="(square, index) in gameBoard"
        @click.once="handleClick(index)"
      >
        <h2 v-if="square.checkedO">O</h2>
        <h2 v-if="square.checkedX">X</h2>
      </div>
    </div>

    <p v-if="playerX.turn">{{ playerX.playerName }}</p>
    <p v-if="playerO.turn">{{ playerO.playerName }}</p>
  </div>
</template>
<style scoped>
.gamePage {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  height: 80vh;
  width: 80vw;
  border-radius: 10px;
  background-color: rgba(47, 121, 92, 0.568);
}
.infoPanel {
  padding: 10px;
}
.players {
  background-color: rgba(139, 0, 139, 0.386);
  width: 180px;
  height: fit-content;
  border-radius: 7px;
  margin: 10px;
}
.playing {
  background-color: rgba(139, 0, 139, 0.621);
}
.winner {
  background-color: rgba(0, 0, 0, 0.567);
  width: 180px;
  height: 100px;
  z-index: 1;
  border-radius: 7px;
  color: antiquewhite;
  margin: 10px;
  display: flex;
  justify-content: center;
  align-items: center;
}
.gameContainer {
  display: grid;
  grid-template-columns: auto auto auto;
  width: 306px;
  height: fit-content;
  /* border: solid 1px rgb(239, 249, 224); */
  padding: 0;
  margin: 80px;
}
.gameSquare {
  border: solid 1px rgba(47, 121, 92, 0.568);
  background-color: rgb(239, 249, 224);
  height: 100px;
  width: 100px;
  margin: 0;
  display: flex;
  align-items: center;
  justify-content: center;
}
button {
  background-color: rgb(164, 100, 63);
  margin: 20px;
  color: rgb(244, 244, 233);
  height: fit-content;
}
</style>
