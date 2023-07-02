<script setup lang="ts">
import { ref, onMounted } from "vue";
import WorldTile from "./WorldTile.vue";
import { TileType } from "./WorldTile.vue";

const tileWidth = "3vw";

const props = defineProps<{
  numOfColumns: number
  numOfRows: number
  numOfTiles: number
}>()
const mapTiles = ref([] as typeof WorldTile[]);

const setMapEvents = () => {
  mapTiles.value.forEach((tile) => {
    if (Math.random() <= 0.45) {
      tile.setEvent();
    }
  })
};

const setMapWalls = () => {
  mapTiles.value.forEach((tile) => {
    if (!tile.hasEvent && Math.random() <= 0.2 ) {
      tile.setWall();
    }
  })
};

onMounted(() => {
  setMapEvents();
  setMapWalls();
})

defineExpose({
  mapTiles
})
</script>



<template>
  <div class="map-container" >
    <WorldTile v-for="(item, index) in numOfTiles" :tileWidth="tileWidth" :key="item" ref="mapTiles"></WorldTile>
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
.map-container:focus {
  outline: 0px;
}
</style>
