<script setup lang="ts">
import HistoryCase from "~/components/blocks/HistoryCase.vue";

interface HistoryItem {
  id: number;
  active: boolean;
}

const items: HistoryItem[] = [
  { id: 1, active: true },
  { id: 2, active: false },
  { id: 3, active: false },
  { id: 4, active: true },
  { id: 5, active: false },
  { id: 6, active: false },
];
</script>

<template>
  <div class="history">
    <div class="history__track">
      <HistoryCase v-for="item in items" :key="item.id" :item="item" />
    </div>
  </div>
</template>

<style lang="scss" scoped>
.history {
  overflow: hidden;
  padding-top: 2rem;
  display: flex;
  align-items: center;
  gap: 1rem;

  &__track {
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
