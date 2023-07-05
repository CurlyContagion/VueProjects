<script lang="ts">
import { Biome } from './GameData.vue';

const colorCodes = {
  YELLOW: "#fcf003",
  BLUE:"#031cfc",
  RED: "#FF0000",
  BLACK: "#000000",
  CYAN: "#21a6db",
  PURPLE: "#8313cf",
  BRIGHTCYAN: "#8cd7f5",
  PINK: "#eb1ceb",
  DEFAULT: "#ffffff",
}

export enum EnemyId {
  ForestGoblinFighter,
  ForestWolfhound,
  ForestGreatStag,
}

export enum SpellId {
  Slam,
  ClubShot,
  Trip
}

export enum StatusId {
  Stunned,
  Poisoned
}

export enum DamageType {
  None,
  True,
  Physical,
  Fire,
  Shadow
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

export class Status {
  id: StatusId;
  name: string;
  duration: number;
  isPassive: boolean;
  onTurnStartEffect?: Function;
  onTurnEndEffect?: Function;

  constructor(config: StatusConfig) {
    this.id = config.id;
    this.name = config.name;
    this.duration = config.baseDuration;
    this.isPassive = config.isPassive;
    this.onTurnStartEffect = config.onTurnStartEffect;
    this.onTurnEndEffect = config.onTurnEndEffect;
  }

}

export interface StatusConfig {
  id: StatusId,
  name: string,
  baseDuration: number,
  isPassive: boolean,
  onTurnStartEffect?: Function,
  onTurnEndEffect?: Function
}

interface effectFunction {
  (target: Enemy, self?: Enemy): void;
}

export interface SpellConfig {
  id: SpellId,
  name: string,
  tooltip: string,
  effect: effectFunction
}

//spells must go in the same order that they are listed in the enum
const spells: SpellConfig[] = [
  {
    id: SpellId.Slam,
    name: "Slam",
    tooltip: `Slam your body into the target, dealing ${colorCodes.YELLOW}115% physical damage${colorCodes.DEFAULT}. Deals an additional flat ${colorCodes.YELLOW}25% damage${colorCodes.DEFAULT} to enemies who are stunned.`,
    effect: (target: Enemy, self?: Enemy) => {
      let damageToDeal = 115;
      if (target.statuses.find(status => status.id === StatusId.Stunned) != undefined) {
        console.log("Slam dealt extra damage because target was stunned!");
        damageToDeal += 25;
      }
      dealDamage(DamageType.Physical, damageToDeal, target);
    }
  },
  {
    id: SpellId.ClubShot,
    name: "Clubsmash",
    tooltip: `Wreck the target with your blunt weapon, dealing ${colorCodes.PINK}90% true damage${colorCodes.DEFAULT} and giving a ${colorCodes.CYAN}10% chance to stun${colorCodes.DEFAULT}.`,
    effect: (target: Enemy, self?: Enemy) => {
      dealDamage(DamageType.True, 90, target);
    }
  },
]

const dealDamage = (damageType: DamageType, damage: number, target: Enemy) => {

}
  
export class Enemy {
  img: string;
  name: string;
  rank: Rank;
  maxHealth: number;
  currentHealth: number;
  physicalDmg: number;
  statuses: Status[];
  spells: SpellConfig[];
  
  constructor(config: EnemyConfig) {
    this.rank = Rank.Normal;
    this.img = config.img;
    this.name = config.name;
    this.maxHealth = config.maxHealth;
    this.physicalDmg = config.physicalDmg;
    this.currentHealth = this.maxHealth;
    this.statuses = [];
    this.spells = config.spells;
  }
}

export interface EnemyConfig {
  id: EnemyId,
  img: string,
  name: string,
  maxHealth: number,
  physicalDmg: number,
  biomes: Biome[],
  spells: SpellConfig[]
}

export let forestEnemyList: EnemyConfig[] = [];
export let desertEnemyList: EnemyConfig[] = [];

const addToEnemyList = ((configs: EnemyConfig[]) => {
  forestEnemyList = configs.filter(config => config.biomes.includes(Biome.Forest));
  desertEnemyList = configs.filter(config => config.biomes.includes(Biome.Desert));
})

addToEnemyList([{
  id: EnemyId.ForestGoblinFighter,
  img: "/img/entities/ForestGoblinThief.png",
  name: "Forest Goblin Fighter",
  maxHealth: 100,
  physicalDmg: 10,
  biomes: [Biome.Forest],
  spells: [spells[SpellId.Slam], spells[SpellId.ClubShot]]
},
{
  id: EnemyId.ForestWolfhound,
  img : "",
  name : "Forest Wolfhound",
  maxHealth: 100,
  physicalDmg: 10,
  biomes: [Biome.Desert],
  spells: [spells[SpellId.Slam]]
}])

</script>
