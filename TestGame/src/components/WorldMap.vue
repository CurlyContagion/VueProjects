<script setup lang="ts">
import { ref } from "vue";

const tileWidth = "3vw";
const numOfColumns = 9;
const numOfRows = 9;
const numOfTiles = 81;

</script>

<template>
  <div class="map-container" >
      <div class="map-tile" v-for="col in numOfTiles" :key="col">{{ col }}</div>
  </div>
</template>

<style scoped>
.map-container {
  position: absolute;
  top: 8vh;
  left: 30vw;
  background-color: gray;
  border-style: solid;
  border-color: black;
  border-width: 4px;
  width: 40vw;
  max-width: 40vw;
  height: 91.15vh;
  z-index: 1;
  display: grid;
  grid-template-columns: repeat(v-bind(numOfColumns), v-bind(tileWidth));
  gap: 2.7vh 1.35vw 
}
.map-tile {
  left: 0.85vw;
  top: 1vh;
  width: v-bind(tileWidth);
  height: 5vh;
  border-style: dashed;
  border-width: 3px;
  border-color: black;
  position:relative;
  text-align: center;
  color: red;
}

</style>
