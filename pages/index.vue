<script setup lang="ts">
import HistoryCase from "~/components/blocks/HistoryCase.vue";
import Carousel from "~/components/blocks/Carousel.vue";
import Header from "~/components/layouts/Header.vue";
import Roulette from "~/components/blocks/Roulette.vue";
import ButtonComponent from "~/components/ui-kit/ButtonComponent.vue";

const rouletteRef = ref<InstanceType<typeof Roulette> | null>(null)
const carouselRef = ref<InstanceType<typeof Carousel> | null>(null)

function startRoulette() {
  rouletteRef.value?.startSpin()
  carouselRef.value?.next()
}
</script>
<template>
  <div class="home">
    <!-- История-->
    <div class="home__history">
      <div class="home__history-track">
        <HistoryCase />
        <HistoryCase />
        <HistoryCase />
        <HistoryCase />
        <HistoryCase />
        <HistoryCase />
      </div>
    </div>
    <div class="container">
      <Header />
    </div>
    <div class="home__cards">
      <Carousel ref="carouselRef" />
    </div>
    <div class="home__roulette">
      <div class="home__roulette-item">
        <Roulette ref="rouletteRef" />
      </div>
    </div>
    <div class="home__buttons">
      <ButtonComponent @click="startRoulette" background="color-orange">
        <h3>Крутить рулетку</h3>
      </ButtonComponent>
    </div>
  </div>
</template>
<style lang="scss" scoped>
.home {
  &__history {
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
  &__cards {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }
  &__roulette {
    position: relative;
    width: 100%;
    height: 27.5rem;
    display: flex;
    align-items: center;
    justify-content: center;
    &-item {
      position: absolute;
      top: -2rem;
    }
  }
  &__buttons {
    display: flex;
    flex-direction: column;
    align-items: center;
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