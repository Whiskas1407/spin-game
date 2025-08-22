<script setup lang="ts">
import { ref, onMounted, watch, nextTick } from 'vue';
import Carousel from "~/components/blocks/Carousel.vue";
import Header from "~/components/layouts/Header.vue";
import Roulette from "~/components/blocks/Roulette.vue";
import HistoryBlock from "~/components/blocks/HistoryBlock.vue";

const rouletteRef = ref<InstanceType<typeof Roulette> | null>(null);
const carouselRef = ref<InstanceType<typeof Carousel> | null>(null);
const statusButton = ref(1)

function startRoulette() {
  rouletteRef.value?.startSpin();
  carouselRef.value?.next();
  statusButton.value = 2
  setTimeout(() => statusButton.value = 1, 3000)
}

const rouletteBtns = ref<HTMLElement[]>([]);
const borderStyle = ref<Record<string, string>>({ transform: 'translateX(0)' });

const rouletteCountList = [1, 3, 5];
const countRoulette = ref<number>(1);

function changeActiveRulette(id: number): void {
  countRoulette.value = id;
}

const updateBorderPosition = () => {
  if (!rouletteBtns.value.length) return;

  const index = rouletteCountList.indexOf(countRoulette.value);
  if (index === -1) return;

  const btnWidth = rouletteBtns.value[0].offsetWidth;
  const offset = index * btnWidth;

  borderStyle.value.transform = `translateX(${offset}px)`;
};

onMounted(async () => {
  await nextTick();
  rouletteBtns.value = Array.from(document.querySelectorAll('.home__buttons-item'));
  updateBorderPosition();
});

watch(countRoulette, () => {
  updateBorderPosition();
});
</script>

<template>
  <div class="home">
    <div class="container">
      <Header />
    </div>

    <div class="home__cards">
      <Carousel ref="carouselRef" />
    </div>

    <div class="home__roulette">
      <div class="home__roulette-item">
        <Roulette ref="rouletteRef" :count="countRoulette" />
      </div>
    </div>

    <div class="home__buttons container">
      <div class="home__buttons-border" :style="borderStyle"></div>
      <h3
          v-for="count of rouletteCountList"
          :key="count"
          @click="changeActiveRulette(count)"
          :class="countRoulette === count ? 'home__buttons-item home__buttons-item-active' : 'home__buttons-item'"
      >
        {{ count }}x
      </h3>
    </div>
    <div class="home__block container">
      <div class="home__spin" @click="startRoulette">
        <img class="home__spin-image-left" src="/image/spin-image-1.png" alt="spin" />
        <h3 class="home__spin-text" v-show="statusButton === 1">КРУТИТЬ за 40 ⭐</h3>
        <h3 class="home__spin-text" v-show="statusButton === 2">Крутим...</h3>
        <img class="home__spin-image-right" src="/image/spin-image-2.png" alt="spin" />
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.home {
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
    position: relative;
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: var(--color-black-opacity-40);
    border-radius: 2rem;
    padding: 0.5rem;

    &-item {
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 3.2rem;
      width: 18.1rem;
      height: 8rem;
      border-radius: 2rem;
      cursor: pointer;
      transition: font-size 0.3s ease;

      &-active {
        font-size: 4rem;
      }
    }

    &-border {
      position: absolute;
      top: 0.5rem;
      left: 2.1rem;
      width: 18.1rem;
      height: 8rem;
      border-radius: 2rem;
      border: 0.2rem solid var(--color-light-blue);
      transition: transform 0.7s ease;
      pointer-events: none;
    }
  }
  &__block {
    padding-top: 1.3rem;
  }
  &__spin {
    position: relative;
    display: flex;
    align-items: center;
    justify-content: center;
    width: 100%;
    border-radius: 2rem;
    height: 10rem;
    background: linear-gradient(89.92deg, #01D9FF 0.06%, #0051FF 102.24%);
    transition: .5s ease;
    &-text {
      font-size: 3.2rem;
      transition: .5s ease;
    }
    &-image {
      &-left {
        width: 12.4rem;
        position: absolute;
        bottom: 0;
        left: 0;
      }
      &-right {
        width: 12.7rem;
        position: absolute;
        top: 0;
        right: 0;
      }
    }
  }
}

</style>
