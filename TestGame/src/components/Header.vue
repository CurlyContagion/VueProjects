<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "Header",
  data() {
    return {
      numOfHeaderButtons: 3,
    };
  },
  props: {
    disabledButtons: {
      type: [] as boolean[],
      required: true,
    },
  },
});
</script>

<template>
  <div>
    <button
      v-for="(value, key) in numOfHeaderButtons"
      :disabled="disabledButtons[key]"
    >
      Long Header Button {{ value }}
    </button>
  </div>
</template>

<style scoped>
button {
  background-color: aqua;
}
</style>
