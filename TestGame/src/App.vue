<script lang="ts">
import MonsterData from "./assets/MonsterData.json";
import type InfoBoxVue from "./components/PlayerPanel.vue";

export enum EntityType {
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

enum EquipmentSlot {
  Head,
  Chest,
  Legs,
  Boots
}

enum ItemRank {
  Basic,
  Improved,
  Epic
}

class Equipment {
  name: String;
  slot: EquipmentSlot;

  constructor(iLevel: Number, itemRank: ItemRank) {
    this.name = "Test";
    this.slot = EquipmentSlot.Head;
  }
}

</script>

<script setup lang="ts">
import Header from "./components/Header.vue";
import EntityTile from "./components/EntityTile.vue";
import PlayerPanel from "./components/PlayerPanel.vue";
import EnvironmentPanel from "./components/EnvironmentPanel.vue";
import WorldMap from "./components/WorldMap.vue";
import { ref, reactive, onMounted, defineComponent } from "vue";

const player = reactive({
  maxHealth: 100,
  currentHealth: 100,
})

const count = ref(1);
const headerButtonsDisabled = ref([false, false, true]);
const currentEnemies = ref([Enemy, Enemy, Enemy, Enemy]);
const tiles = ref([] as (typeof EntityTile)[]);

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

onMounted(() => {
  dynamicComp.props = {
    title: "Info Box"
  }
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
  <WorldMap/>
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
