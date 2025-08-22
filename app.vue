<template>
  <NuxtLayout>
    <div class="app">
      <img class="app__shadow-top" src="/image/shadow-top.svg" alt="shadow">
      <div class="app__container">
        <NuxtPage />
      </div>
      <img class="app__shadow-bottom" src="/image/shadow-bottom.svg" alt="shadow">
      <Menu />
    </div>
  </NuxtLayout>
</template>
<style lang="scss" scoped>
.app {
  display: flex;
  flex-direction: column;
  align-items: center;
  position: relative;
  padding-top: 2rem;
  &__container {
    position: relative;
    z-index: 1;
  }
  &__shadow {
    &-top {
      position: absolute;
      top: 0;
      z-index: -1;
    }
    &-bottom {
      position: absolute;
      bottom: 0;
      z-index: -1;
    }
  }
}
</style>
<script setup lang="ts">
import Menu from "~/components/layouts/Menu.vue";
</script>