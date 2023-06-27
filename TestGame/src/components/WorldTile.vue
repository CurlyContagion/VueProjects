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

</script>

<script setup lang="ts">
  import { ref, computed} from "vue"


  const hasEvent = ref(false);
  const tileType = ref(TileType.Empty)

  const setEvent = () => {
    hasEvent.value = true;
    const num = Math.random()
    switch (true) {
        case (num < 0.3):
          tileType.value = TileType.Stash
          break;
        case (num < 0.4):
          tileType.value = TileType.Rest
          break;
        case (num < 0.6):
          tileType.value = TileType.MonsterEncounter
          break;
        case (num < 0.7):
          tileType.value = TileType.QuestEncounter
          break;
        case (num < 0.8):
          tileType.value = TileType.Shop
          break;
        case (num < 0.95):
          tileType.value = TileType.Exit
          break;
        case (num < 1):
          tileType.value = TileType.BossEncounter
          break;
      default: 
    }
  }

  const tileStyling = computed(() => ({
  "background-color": hasEvent.value ? "goldenrod" : "lightgray"
}))

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

  const props = defineProps<{
    tileWidth: String
  }>()

  defineExpose({
    hasEvent,
    tileType,
    setEvent
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

