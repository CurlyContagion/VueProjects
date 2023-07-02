
<script setup lang="ts">
import Header from "./components/Header.vue";
import EntityTile from "./components/EntityTile.vue";
import PlayerPanel from "./components/PlayerPanel.vue";
import EnvironmentPanel from "./components/EnvironmentPanel.vue";
import WorldMap from "./components/WorldMap.vue";
import {  Equipment } from "./GameData.vue";
import { ref, reactive, onMounted, watch } from "vue";

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
const playerPosition = ref(0)

const count = ref(1);
const headerButtonsDisabled = ref([false, false, true]);
const numOfColumns = 9;
const numOfRows = 9;
const numOfTiles = numOfColumns * numOfRows;

const map = ref(null);
let mapTiles = [];

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

const dynamicComp = reactive({
  is: PlayerPanel as any,
  props: {}
})

const change = function() {
  console.log(dynamicComp.is)
  if (dynamicComp.is.__name == "PlayerPanel") {
    dynamicComp.props = {
      styleTop: "0px",
      styleLeft: "0px"
    }
    dynamicComp.is = EntityTile
  }
  else {
    dynamicComp.props = {
      title: "Info Box"
    }
    dynamicComp.is = PlayerPanel
  }
}

const attemptToUpdatePosition = (newPosition: number) => {
  if (newPosition < 0 || newPosition > numOfTiles - 1 || (newPosition === playerPosition.value + 1 && playerPosition.value % numOfColumns === numOfColumns - 1 ) || (newPosition === playerPosition.value - 1 && playerPosition.value % numOfColumns === 0 ) || mapTiles[newPosition].isWall === true) {
    console.log("Invalid move attempted!")
    return;
  }
  mapTiles[playerPosition.value].hasPlayer = false;
  playerPosition.value = newPosition
  mapTiles[newPosition].hasPlayer = true;
}

onMounted(() => {
  dynamicComp.props = {
    title: "Info Box"
  }
  setTimeout(() => {
    mapTiles = map.value.mapTiles;
    mapTiles[playerPosition.value].hasPlayer = true;
    attemptToUpdatePosition(4);
}, 500)
})

</script>

<template>
  <Header :disabledButtons="headerButtonsDisabled" />
  <h1>Hello {{ count }}</h1>
  <button @click="change()">Increment</button>
  <button @click="">Activate</button>
  <button @click="">Deactivate</button>
  <PlayerPanel/>
  <!--<KeepAlive>
    <component :is="dynamicComp.is" v-bind="dynamicComp.props" />
  </KeepAlive>-->
    <WorldMap class=".world-map" tabindex="0" @keydown="movePlayer($event)" ref="map" :numOfColumns="numOfColumns" :numOfRows="numOfRows" :numOfTiles="numOfTiles"/>
  <EnvironmentPanel/>
</template>

<style>
body {
  color: white;
  background-color: black;
  background-image: url("/img/environments/DesertBackground.png");
  background-size: 100vw 100vh;
  overflow: hidden;
}
</style>
