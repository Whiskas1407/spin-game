<template>
  <NuxtLayout>
    <div class="app">
      <img class="app__shadow-top" src="/image/shadow-top.svg" alt="shadow">
      <div class="app__container">
        <NuxtPage />
      </div>
<!--      <img class="app__shadow-bottom" src="/image/shadow-top.svg" alt="shadow">-->
    </div>
  </NuxtLayout>
</template>
<style lang="scss" scoped>
.app {
  display: flex;
  flex-direction: column;
  align-items: center;
  &__container {
    position: relative;
    z-index: 1;
  }
  &__shadow {
    &-top {
      position: fixed;
      top: 0;
      z-index: 0;
    }
    &-bottom {
      position: fixed;
      bottom: 0;
      z-index: 0;
    }
  }
}
</style>
<script setup lang="ts">
</script>