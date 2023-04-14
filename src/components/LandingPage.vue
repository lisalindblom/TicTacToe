<script setup lang="ts">
import { ref } from "vue";
import AddUser from "./AddPlayer.vue";
import TicTac from "./TicTac.vue";
import { Player } from "../models/Player";

let playerX = ref();
let playerO = ref();

let show = ref(true);

let playerXPresentation = ref();
let playerOPresentation = ref();

const message = (playerNameX: string, playerNameO: string) => {
  playerXPresentation.value = playerNameX + " spelar som X";
  playerOPresentation.value = playerNameO + " spelar som O";
};

const addPlayer = (playerNameX: string, playerNameO: string) => {
  playerX.value = new Player(playerNameX, false);
  playerO.value = new Player(playerNameO, false);
  console.log(playerO.value, playerX.value);

  show.value = !show.value;
  message(playerNameX, playerNameO);
};
</script>

<template>
  <AddUser v-if="show" @start-game="addPlayer"></AddUser>
  <TicTac :player-o="playerO" :player-x="playerX" v-else></TicTac>
</template>

<style scoped></style>
