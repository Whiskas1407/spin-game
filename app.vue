<script setup lang="ts">
import {useRoute} from "#vue-router";
import Menu from "~/components/layouts/Menu.vue";
import HistoryBlock from "~/components/blocks/HistoryBlock.vue";
const route = useRoute()
</script>
<template>
  <NuxtLayout>
    <div class="app">
      <img class="app__shadow-top" src="/image/shadow-top.svg" alt="shadow">
      <div class="app__container">
        <HistoryBlock v-if="route.fullPath !== '/leaders'" />
        <NuxtPage />
        <Menu />
      </div>
      <img class="app__shadow-bottom" src="/image/shadow-bottom.svg" alt="shadow">
    </div>
  </NuxtLayout>
</template>
<style lang="scss" scoped>
.app {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  position: relative;
  &__container {
    position: relative;
    z-index: 1;
    max-width: 100%;
    margin: 0 auto;
  }
  &__shadow {
    &-top {
      width: 100vw;
      position: absolute;
      top: 0;
      z-index: -1;
    }
    &-bottom {
      width: 100vw;
      position: absolute;
      bottom: 0;
      z-index: -1;
    }
  }
}
</style>