<script setup lang="ts">
import { useRoute } from "#vue-router";
import Menu from "~/components/layouts/Menu.vue";
import HistoryBlock from "~/components/blocks/HistoryBlock.vue";

const route = useRoute();
</script>

<template>
  <NuxtLayout>
    <div class="app">
      <img class="app__shadow-top" src="/image/shadow-top.svg" alt="shadow" />
      <div class="app__container">
        <HistoryBlock v-if="route.fullPath !== '/leaders'" />
        <NuxtPage />
        <Menu />
      </div>
      </div>
      <img class="app__shadow-bottom" src="/image/shadow-bottom.svg" alt="shadow" />
  </NuxtLayout>
</template>

<style lang="scss" scoped>
.app {
  display: flex;
  flex-direction: column;
  align-items: center;
  position: relative;

  &__container {
    position: relative;
    z-index: 1;
  }

  &__shadow-top,
  &__shadow-bottom {
    width: 58.9rem;
    position: absolute;
    z-index: -1;
  }

  &__shadow-top { top: 0; }
  &__shadow-bottom { bottom: 0; }
}
</style>