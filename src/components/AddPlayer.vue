<script setup lang="ts">
import { ref } from "vue";

let playerX = ref("");
let playerO = ref("");

defineEmits(["startGame"]);
</script>
<template>
  <div class="container">
    <h1>Tre i rad</h1>
    <form @submit.prevent="() => $emit('startGame', playerX, playerO)">
      <label>Spelare X</label>
      <input placeholder=" Fyll i namn" type="text" v-model="playerX" />
      <label>Spelare O</label>
      <input placeholder=" Fyll i namn" type="text" v-model="playerO" />
      <button>Lägg till spelare</button>
    </form>
  </div>
</template>
<style scoped>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-evenly;
  border: 1px solid rgb(158, 155, 155);
  border-radius: 10px;
  height: 80vh;
  width: 50vw;
  background-color: rgba(47, 121, 92, 0.568);
}
h1 {
  color: white;
}
form {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-around;
}
label {
  margin: 10px;
}
input {
  border: 1px solid black;
  border-radius: 5px;
  margin-bottom: 10px;
}
button {
  border: 2px solid rgb(4, 97, 97);
  margin: 30px;
  margin-top: 100px;
}
</style>
