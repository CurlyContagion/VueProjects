<script setup lang="ts">
import { ref } from "vue";

</script>

<template>
  <div class="modal" >
    <p class="modal-title">Player Panel</p>
  </div>
</template>

<style scoped>
.modal {
  top: 8vh;
  left: 0vw;
  position: absolute;
  width: 30vw;
  height: 91.15vh;
  background-color: rgb(10, 59, 43);
  font-family: cursive;
  color: rgb(201, 70, 201);
  z-index: 0;
  border-style: solid;
  border-color: black;
  border-width: 4px;
}
.modal-title {
  text-align: center;
}
</style>
