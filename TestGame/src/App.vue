
<script setup lang="ts">
import Header from "./components/Header.vue";
import PlayerPanel from "./components/PlayerPanel.vue";
import EnvironmentPanel from "./components/EnvironmentPanel.vue";
import WorldMap from "./components/WorldMap.vue";
import WorldTile from "./components/WorldTile.vue"
import {  Equipment, Biome, generateBiomeName } from "./GameData.vue";
import { forestEnemyList, desertEnemyList } from "./MonsterData.vue";
import type { EnemyConfig, Enemy } from "./MonsterData.vue";
import { ref, reactive, onMounted, watch, provide, computed } from "vue";

const player = reactive({
  maxHealth: 100,
  currentHealth: 100,
  weapon: Equipment,
  head: Equipment,
  chest: Equipment,
  legs: Equipment,
  boots: Equipment,
  inventory: [] as Equipment[]
})

const playerPosition = ref(9);
provide('playerPosition', playerPosition);

const currentBiome = ref(Biome.Forest);
const currentBiomeProperties = computed(() => {
  const biomeName = generateBiomeName(currentBiome.value);
  let biomeImg = "";
  let biomeAlt = "";
  let biomeEnemies: EnemyConfig[] = [];
  switch (currentBiome.value) {
    case Biome.Forest:
      biomeImg = "/img/environments/ForestBackground.png";
      biomeAlt = "Forest";
      biomeEnemies = forestEnemyList;
      break;
    case Biome.Desert:
    biomeImg = "/img/environments/DesertBackground.png";
      biomeAlt = "Desert";
      biomeEnemies = desertEnemyList;
      break;
    case Biome.Ruins:
    biomeImg = "/img/environments/RuinsBackground.png";
      biomeAlt = "Ruins";
      break;
  }
  return {
    type: currentBiome.value,
    name: biomeName,
    img: biomeImg,
    alt: biomeAlt,
    enemies: biomeEnemies
  };
})


const count = ref(1);
const headerButtonsDisabled = ref([false, false, true]);
const numOfColumns = 9;
const numOfRows = 9;
const numOfTiles = numOfColumns * numOfRows;

const map = ref<InstanceType<typeof WorldMap> | null>(null);

let mapTiles = [] as InstanceType<typeof WorldTile>[]

const movePlayer = (event: KeyboardEvent) => {
  if (event.key === "w" || event.key === "ArrowUp"){
    console.log("Moved up!")
    attemptToUpdatePosition(playerPosition.value - numOfColumns);
  }
  else if (event.key === "a" || event.key === "ArrowLeft") {
    console.log("Moved left!")
    attemptToUpdatePosition(playerPosition.value - 1);
  }
  else if (event.key === "s" || event.key === "ArrowDown") {
    console.log("Moved down!")
    attemptToUpdatePosition(playerPosition.value + numOfColumns);
  }
  else if (event.key === "d" || event.key === "ArrowRight") {
    console.log("Moved right!")
    attemptToUpdatePosition(playerPosition.value + 1);
  }
  /*else {
    console.log(`${event.key} was pressed, but doesn't map to movement!`)
  }*/
}

watch(playerPosition, (newPosition, oldPosition) => {
  console.log(`Old pos was ${oldPosition}, new pos is ${newPosition}`);
})


const attemptToUpdatePosition = (newPosition: number) => {
  if (newPosition < 0 || newPosition > numOfTiles - 1 || (newPosition === playerPosition.value + 1 && playerPosition.value % numOfColumns === numOfColumns - 1 ) || (newPosition === playerPosition.value - 1 && playerPosition.value % numOfColumns === 0 ) || mapTiles[newPosition].isWall === true) {
    console.log("Invalid move attempted!")
    return;
  }
  playerPosition.value = newPosition
}

const changeBiome = (biome: Biome) => {
  currentBiome.value = biome;
}

onMounted(() => {
  if (map.value != null) {
    mapTiles = map.value.mapTiles;
  }
  playerPosition.value = 40;
})

</script>

<template>
  <Header :disabledButtons="headerButtonsDisabled" />
  <h1>Hello {{ count }}</h1>
  <button class="test" @click="changeBiome(Biome.Desert)">Increment</button>
  <PlayerPanel/>
  <WorldMap class=".world-map" tabindex="0" @keydown="movePlayer($event)" ref="map" :numOfColumns="numOfColumns" :numOfRows="numOfRows" :numOfTiles="numOfTiles" :playerPosition="playerPosition"/>
  <EnvironmentPanel v-bind:="currentBiomeProperties"/>
</template>

<style>
body {
  color: white;
  background-color: black;
  background-image: url("/img/environments/DesertBackground.png");
  background-size: 100vw 100vh;
  overflow: hidden;
}
.test {
  top: 5vh;
  left: 5vw;
  position: absolute;
  color: red;
}
</style>
