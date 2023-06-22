<script setup lang="ts">
import { ref } from "vue";

const numOfHeaderButtons = ref(3)

const props = defineProps<{
  disabledButtons: boolean[]
}>()

</script>

<template>
  <div class="headerBar">
    <button v-for="(value, key) in numOfHeaderButtons" :disabled="disabledButtons[key]">Long Header Button {{ value }} </button>
  </div>
</template>

<style scoped>
button {
  background-color: aqua;
}
button:hover:enabled{
  background: rgb(163, 255, 255);
}
.headerBar {
  position: absolute;
  top: 0px;
  left: 0px;
  width: 100vw;
  height: 8vh;
  background: linear-gradient(0deg, rgb(209, 209, 209), gray);
}
</style>
