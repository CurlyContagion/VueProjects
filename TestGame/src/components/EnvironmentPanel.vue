<script lang="ts">
const generateColoredTooltip = (originalTooltip: string) => {
  let finalTooltip = "";
  let indexToSubstringTo = originalTooltip.indexOf("#");
  if (indexToSubstringTo === -1) {
    return originalTooltip
  }
  let spanContent = originalTooltip.substring(0, indexToSubstringTo);
  finalTooltip += `<span>${spanContent}<span/>`
  originalTooltip = originalTooltip = originalTooltip.substring(indexToSubstringTo);
  for (let i = 0; i<10; i++){
    let spanColor = originalTooltip.substring(0, 7);
    originalTooltip = originalTooltip.substring(7);
    indexToSubstringTo = originalTooltip.indexOf("#");
    if (indexToSubstringTo === -1) {
      finalTooltip += `<span style="color:${spanColor}">${originalTooltip}<span/>`
      break;
    }
    else {
      spanContent = originalTooltip.substring(0, indexToSubstringTo);
      finalTooltip += `<span style="color:${spanColor}">${spanContent}<span/>`
      originalTooltip = originalTooltip.substring(indexToSubstringTo);
    }
  }
  return finalTooltip;
}
</script>

<script setup lang="ts">
import { ref } from "vue";
import { Biome } from "@/GameData.vue";
import type { Enemy, EnemyConfig } from "@/MonsterData.vue";

const props = defineProps<{
    type: Biome
    name: string,
    img: string,
    alt: string,
    enemies: EnemyConfig[]
}>()

const showTooltip = (id: string) => {
  document.getElementById(id).style.display = "block";
}

const hideTooltip = (id: string) => {
  document.getElementById(id).style.display = "none";
}



</script>



<template>
  <div class="panel" >
    <p class="panel-title">Environment Panel</p>
    <img :src="img" :alt="alt" class="biome-image"/>
    <p class="img-name">{{ name }}</p>
    <div class="test" v-html="generateColoredTooltip(enemies[0].spells[0].tooltip)"></div>
    <dl class="enemy-list">
      Possible Encounters:
      <div v-for="(enemy, index) in enemies" :key="index" :tooltip="'Possible Moves:\n' + enemy.name">
        <dd class="enemy-element" >{{ enemy.name }}</dd>
        <ul >
          <li @mouseover="showTooltip(spell.name)" @mouseout="hideTooltip(spell.name)" class="move-element"  v-for="(spell, innerIndex) in enemy.spells" :key="innerIndex"> 
            {{ spell.name }}
            <div :id="spell.name" class="move-tooltip" v-html="generateColoredTooltip(spell.tooltip)"></div>
          </li>
        </ul>
      </div>
    </dl>
    <ul class="enemy-list">Possible Resources:
      <li>Resource 1</li>
    </ul>
  </div>
</template>

<style scoped>
.test{
  color: white;
  position: relative;
  top: -1vh;
  left: 1vw;
  font-size: 0.7rem;
}
.panel {
  top: 8vh;
  left: 70vw;
  position: absolute;
  width: 30vw;
  height: 91.15vh;
  background-image: linear-gradient(180deg, darkblue, rgb(0, 0, 0));
  font-family: cursive;
  z-index: 0;
  border-style: solid;
  border-color: black;
  border-width: 4px;
  color: yellow
}
.panel-title {
  text-align: center;
  font-size: 1.5rem;
  margin-bottom: 0px;
}
.biome-image{
  position: relative;
  top: 2vh;
  left: 5vw;
  width: 20vw;
  height: 30vh;
}
.img-name{
  font-size: 1.2rem;
  color:yellow;
  position: relative;
  text-align: center;
}
.enemy-list {
  position: relative;
  left: 1.5vw;
  background-color: rgb(12, 12, 143);
  width: 24.5vw;
  box-shadow: 0.6vw 0.6vh 0.5vw rgb(4, 4, 102);
}
.enemy-element{
  font-size: 1.4rem;
}
.enemy-element::before{
  left: 0vw;
  content: url("/img/icons/BattleIconOrange.svg");
  width: 1vw;
  height: 1vh;
  position: absolute;
  transform: scale(0.03);
}
.move-tooltip{
  border-style: solid;
  border-color: gray;
  content: attr(tooltip);
  border-radius: 0.75vw;
  width: 18vw;
  background-color: black;
  position: absolute;
  left: 5vw;
  top: -2vh;
  z-index: 1;
  font-size: 0.8rem;
  color: white;
  display: none;
}
.move-element{
  margin-top: 0px;
  position: relative;
  left: 1.5vw;
}
</style>
