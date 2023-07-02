<script lang="ts">

export enum TileType {
  Empty,
  Stash,
  Rest,
  QuestEncounter,
  MonsterEncounter,
  BossEncounter,
  Shop,
  Exit
}

const STASH_CHANCE = 0.20;
const REST_CHANCE = 0.1;
const QUEST_CHANCE = 0.15;
const MONSTER_CHANCE = 0.30;
const BOSS_CHANCE = 0.05;
const SHOP_CHANCE = 0.15;
const EXIT_CHANCE = 0.05;

if (STASH_CHANCE + REST_CHANCE + QUEST_CHANCE + MONSTER_CHANCE + BOSS_CHANCE + SHOP_CHANCE + EXIT_CHANCE != 1) {
  console.error("Encounter chances are not equal to 1!")
  throw new Error;
}
</script>

<script setup lang="ts">
  import { ref, computed, onMounted} from "vue"

  const hasPlayer = ref(false);
  const isWall = ref(false);
  const hasEvent = ref(false);
  const tileType = ref(TileType.Empty)

  const setEvent = () => {
    hasEvent.value = true;
    const num = Math.random()
    switch (true) {
        case (num < STASH_CHANCE):
          tileType.value = TileType.Stash
          break;
        case (num < STASH_CHANCE + REST_CHANCE):
          tileType.value = TileType.Rest
          break;
        case (num < STASH_CHANCE + REST_CHANCE + MONSTER_CHANCE):
          tileType.value = TileType.MonsterEncounter
          break;
        case (num < STASH_CHANCE + REST_CHANCE + MONSTER_CHANCE + QUEST_CHANCE):
          tileType.value = TileType.QuestEncounter
          break;
        case (num < STASH_CHANCE + REST_CHANCE  + MONSTER_CHANCE + QUEST_CHANCE + SHOP_CHANCE):
          tileType.value = TileType.Shop
          break;
        case (num < STASH_CHANCE + REST_CHANCE  + MONSTER_CHANCE + QUEST_CHANCE + SHOP_CHANCE + EXIT_CHANCE):
          tileType.value = TileType.Exit
          break;
        case (num < STASH_CHANCE + REST_CHANCE  + MONSTER_CHANCE + QUEST_CHANCE + SHOP_CHANCE + EXIT_CHANCE + BOSS_CHANCE):
          tileType.value = TileType.BossEncounter
          break;
      default: 
    }
  }

  const setPlayer = () => {
    hasPlayer.value = true;
  }

  const removePlayer = () => {
    hasPlayer.value = false;
  }

  const tileStyling = computed(() => {
    let backgroundColor = ""
    if (hasPlayer.value) {
      backgroundColor = "blue";
    }
    else if (hasEvent.value) {
      backgroundColor = "goldenrod";
    }
    else if (isWall.value) {
      backgroundColor = "black"
    }
    else {
      backgroundColor = "lightgray"
    }
    return {
      "background-color": backgroundColor
    } 
})

  const imgStyling = computed(() => ({
    display: hasEvent.value ? "block" : "none"
  }))

  const tileImg = computed(() => {
    switch (tileType.value) {
      case TileType.Empty:
        return ""
      case TileType.Stash:
        return "/img/icons/ChestIcon.svg"
      case TileType.Rest:
        return "/img/icons/CampfireIcon.svg"
      case TileType.QuestEncounter: 
        return "/img/icons/ExclamationMark.svg"
      case TileType.MonsterEncounter:
        return "/img/icons/BattleIcon.svg";
      case TileType.BossEncounter:
        return "/img/icons/SkullIcon.svg"
      case TileType.Shop:
        return "/img/icons/TradeIcon.svg"
      case TileType.Exit:
        return "/img/icons/ExitIcon.svg"
      default:
        return ""
    }
  })

  const setWall = () => {
    isWall.value = true;
  }

  const props = defineProps<{
    tileWidth: string
  }>()

  defineExpose({
    hasEvent,
    tileType,
    hasPlayer,
    isWall,
    setEvent,
    setWall
  })
</script>

<template>
  <div class="map-tile" :style="tileStyling">
    <img :src="tileImg" alt="Battle Tile" :style="imgStyling"/>
  </div>
</template>

<style>
.map-tile {
  left: 0.85vw;
  top: 1vh;
  width: v-bind(tileWidth);
  height: 5vh;
  border-style: dashed;
  border-width: 3px;
  border-color: black;
  position:relative;
  text-align: center;
  color: red;
}
.map-tile > img {
  position: relative;
  width: inherit;
  height: inherit;
}
</style>

