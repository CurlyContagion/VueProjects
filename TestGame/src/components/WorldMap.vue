<script setup lang="ts">
import { ref, onMounted } from "vue";
import WorldTile from "./WorldTile.vue";
import { TileType } from "./WorldTile.vue";

const tileWidth = "3vw";
const numOfColumns = 9;
const numOfRows = 9;
const numOfTiles = numOfRows * numOfColumns; //81

const mapTiles = ref([] as typeof WorldTile[]);

const setMapEvents = () => {
  mapTiles.value.forEach((tile) => {
    if (Math.random() <= 0.45) {
      tile.setEvent();
    }
  })
};

onMounted(() => {
  setMapEvents();
})
</script>

<template>
  <div class="map-container" >
    <WorldTile v-for="col in numOfTiles" :tileWidth="tileWidth" :key="col" ref="mapTiles"></WorldTile>
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
</style>
