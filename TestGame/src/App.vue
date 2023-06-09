<script lang="ts">
  import { defineComponent, onMounted } from 'vue';
  import Header from "./components/Header.vue";
  import EntityTile from "./components/EntityTile.vue";
  import MonsterData from "./assets/MonsterData.json";

  enum Environment {
    Forest
  }

  enum EntityType {
    ForestGoblinFighter,
    ForestWolfhound,
    ForestGreatStag
  }

  enum Rank {
    Normal,
    Veteran,
    Elite,
    Boss
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

  export default defineComponent({
    data() {
      return {
        count: 1,
        headerButtonsDisabled: [false, false, true],
        currentEnemies: [Enemy, Enemy, Enemy, Enemy],
        tiles: [] as any[],
        refNames: ["TileOne", "TileTwo", "TileThree", "TileFour"]
      }
    },
    methods: {
      increment() {
        this.tiles.forEach(tile => {
          tile.enemy = new Enemy(EntityType.ForestGoblinFighter);
          tile.activateTile();
        });
      },
      activate() {
        this.tiles.forEach(tile => {
          tile.activateTile();
        });
      },
      deactivate() {
        this.tiles.forEach(tile => {
          tile.deactivateTile();
        });
      }
    },
    components: {
      Header,
      EntityTile
    },
    mounted() {
      this.tiles.push(this.$refs.TileOne);
      this.tiles.push(this.$refs.TileTwo);
      this.tiles.push(this.$refs.TileThree);
      this.tiles.push(this.$refs.TileFour);
      this.tiles.forEach((tile) => {
        tile.deactivateTile();
      });
    }
  });
  
</script>

<template>
    <Header :disabledButtons="headerButtonsDisabled"/>
    <h1>Hello {{ count }}</h1>
    <button @click="increment()">Increment</button>
    <button @click="activate()">Activate</button>
    <button @click="deactivate()">Deactivate</button>
    <EntityTile styleLeft="650px" styleTop="50px"  ref="TileOne"/>
    <EntityTile styleLeft="1200px" styleTop="50px"  ref="TileTwo"/>
    <EntityTile styleLeft="650px" styleTop="450px"  ref="TileThree"/>
    <EntityTile styleLeft="1200px" styleTop="450px"  ref="TileFour"/>
</template>

<style>
  body {
    color:white;
    background-color: black;
    background-image: url("/img/environments/ForestBackground.png");
  }
</style>
