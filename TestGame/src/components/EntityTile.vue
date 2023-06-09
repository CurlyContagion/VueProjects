<script lang="ts">
  import { defineComponent } from 'vue';
  import { Enemy } from "../App.vue";

  export default defineComponent({
    name: "EntityTile",
    data() {
      return {
        isActiveTile: true,
        enemy: Enemy
      }
    },
    methods: {
      activateTile() {
        console.log("Tile Activated!");
        this.isActiveTile = true;
        this.$refs.innerImg.style.display="inline";
      },
      deactivateTile() {
        this.isActiveTile = false;
        this.$refs.innerImg.style.display="none";
      }
    },
    props: {
      styleTop: String,
      styleLeft: String
    }
  });
  
</script>

<template>
  <div class="battleTile">
    <img class="battleTileInner" v-show="isActiveTile" :src="enemy.img" :alt="enemy.imgAlt" ref="innerImg"/>
  </div>
  
</template>

<style scoped>
    .battleTile {
        position: absolute;
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
