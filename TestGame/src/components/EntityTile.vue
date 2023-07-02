<script setup lang="ts">
  import { ref } from 'vue';
  import { Enemy, EntityType } from "../GameData.vue";

  const isActiveTile = ref(true);
  const enemy = new Enemy(EntityType.ForestGoblinFighter);
  const props = defineProps<{
    styleTop: String,
    styleLeft: String
  }>()
  
</script>

<template>
  <div class="battleTile">
    <img class="battleTileInner" v-show="isActiveTile" :src="enemy.img" :alt="enemy.imgAlt" ref="innerImg"/>
  </div>
  
</template>

<style scoped>
    .battleTile {
        position: relative;
        top:v-bind(styleTop);
        left:v-bind(styleLeft);
        width: 400px;
        height: 300px;
        border-image: linear-gradient(rgb(230, 14, 14), rgb(207, 100, 0)) 30;
        border-style: solid;
        border-width: 6px;
        border-color: darkred;
    }
    .battleTileInner {
      width: 400px;
      height: 300px;
    }
    .position-one {
      
    }
    .position-two {

    }
    .position-three {

    }
    .position-four {

    }
</style>
