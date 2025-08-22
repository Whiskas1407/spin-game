<script setup lang="ts">
import HistoryCase from "~/components/blocks/HistoryCase.vue";
</script>
<template>
  <div class="history">
    <div class="history-track">
      <HistoryCase v-for="i of 6" key="i" />
    </div>
  </div>
</template>
<style lang="scss" scoped>
.history {
  display: flex;
  align-items: center;
  gap: 1rem;
  width: 100vw;
  overflow: hidden;

  &-track {
    display: flex;
    gap: 1rem;
    animation: scroll-left 10s linear infinite;
  }
}
@keyframes scroll-left {
  0% {
    transform: translateX(0);
  }
  100% {
    transform: translateX(-50%);
  }
}
</style>