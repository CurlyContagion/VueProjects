<script lang="ts">
import MonsterData from "./assets/MonsterData.json";

enum Environment {
  Forest,
}

enum EntityType {
  ForestGoblinFighter,
  ForestWolfhound,
  ForestGreatStag,
}

enum Rank {
  Normal,
  Veteran,
  Elite,
  Boss,
}

export class Enemy {
  img: String;
  imgAlt: String;
  rank: Rank;
  maxHealth: Number;
  currentHealth: Number;
  physicalDmg: Number;

  constructor(type: EntityType) {
    this.rank = Rank.Normal;
    this.img = MonsterData[type].img;
    this.imgAlt = MonsterData[type].imgAlt;
    this.maxHealth = MonsterData[type].maxHealth;
    this.physicalDmg = MonsterData[type].physicalDmg;
    this.currentHealth = this.maxHealth;
  }
}
</script>

<script setup lang="ts">
import Header from "./components/Header.vue";
import EntityTile from "./components/EntityTile.vue";
import { ref, onMounted } from "vue";

const count = ref(1);
const headerButtonsDisabled = ref([false, false, true]);
const currentEnemies = ref([Enemy, Enemy, Enemy, Enemy]);
const tiles = ref([] as (typeof EntityTile)[]);

const TileOne = ref(null);
const TileTwo = ref(null);
const TileThree = ref(null);
const TileFour = ref(null);

const increment = function () {
  tiles.value.forEach((tile) => {
    tile.enemy = new Enemy(EntityType.ForestGoblinFighter);
    tile.activateTile();
  });
};

const activate = function () {
  tiles.value.forEach((tile) => {
    tile.activateTile();
  });
};

const deactivate = function () {
  tiles.value.forEach((tile) => {
    tile.deactivateTile();
  });
};

onMounted(() => {
  console.log(TileOne.value);
  tiles.value.push(TileOne.value);
  tiles.value.push(TileTwo.value);
  tiles.value.push(TileThree.value);
  tiles.value.push(TileFour.value);
  tiles.value.forEach((tile) => {
    tile.deactivateTile();
  });
});
</script>

<template>
  <Header :disabledButtons="headerButtonsDisabled" />
  <h1>Hello {{ count }}</h1>
  <button @click="increment()">Increment</button>
  <button @click="activate()">Activate</button>
  <button @click="deactivate()">Deactivate</button>
  <EntityTile
    styleLeft="650px"
    styleTop="50px"
    ref="TileOne"
  />
  <EntityTile
    styleLeft="1200px"
    styleTop="50px"
    ref="TileTwo"
  />
  <EntityTile
    styleLeft="650px"
    styleTop="450px"
    ref="TileThree"
  />
  <EntityTile
    styleLeft="1200px"
    styleTop="450px"
    ref="TileFour"
  />
</template>

<style>
body {
  color: white;
  background-color: black;
  background-image: url("/img/environments/ForestBackground.png");
}
</style>
