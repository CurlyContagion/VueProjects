<script lang="ts">
import MonsterData from "./assets/MonsterData.json"

export enum EntityType {
    ForestGoblinFighter,
    ForestWolfhound,
    ForestGreatStag,
}

export enum Tier {
  I,
  II,
  III,
  IV,
  V
}
  
export enum Rank {
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
  
    constructor(type: EnemyConfig) {
      this.rank = Rank.Normal;
      this.img = type.img;
      this.imgAlt = type.imgAlt;
      this.maxHealth = type.maxHealth;
      this.physicalDmg = type.physicalDmg;
      this.currentHealth = this.maxHealth;
  }
}

interface EnemyConfig {
  img: String,
  imgAlt: String,
  maxHealth: Number,
  physicalDmg: Number,
}

const ForestGoblinFighterConfig: EnemyConfig = {
  img : "/img/entities/ForestGoblinThief.png",
  imgAlt : "Forest Goblin Fighter",
  maxHealth: 100,
  physicalDmg: 10,
}

const ForestWolfhoundConfig: EnemyConfig = {
  img : "",
  imgAlt : "Forest Wolfhound",
  maxHealth: 100,
  physicalDmg: 10,
}

</script>
