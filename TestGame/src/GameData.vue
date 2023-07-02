<script lang="ts">
import MonsterData from "./assets/MonsterData.json"

export enum EntityType {
    ForestGoblinFighter,
    ForestWolfhound,
    ForestGreatStag,
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
  
    constructor(type: EntityType) {
      this.rank = Rank.Normal;
      this.img = MonsterData[type].img;
      this.imgAlt = MonsterData[type].imgAlt;
      this.maxHealth = MonsterData[type].maxHealth;
      this.physicalDmg = MonsterData[type].physicalDmg;
      this.currentHealth = this.maxHealth;
    }
}
  export enum EquipmentSlot {
    Head,
    Chest,
    Legs,
    Boots
  }
  
export enum ItemRank {
    Basic,
    Improved,
    Epic
  }
  
export class Equipment {
    name: String;
    slot: EquipmentSlot;
  
    constructor(iLevel: Number, itemRank: ItemRank) {
      this.name = "Test";
      this.slot = EquipmentSlot.Head;
    }
  }

</script>
