<script setup lang="ts">
import { ref, onMounted, watch, nextTick } from 'vue';
import Carousel from "~/components/blocks/Carousel.vue";
import Header from "~/components/layouts/Header.vue";
import Roulette from "~/components/blocks/Roulette.vue";

const carouselRef = ref<InstanceType<typeof Carousel> | null>(null);
const statusButton = ref(1);

// Количество рулеток и их референсы
const rouletteCountList = [1, 3, 5];
const countRoulette = ref<number>(1);
const roulettes = ref([{ id: 'r1' }]);
const rouletteRefs = ref<InstanceType<typeof Roulette>[]>([]);

function setRef(el: InstanceType<typeof Roulette> | null, index: number) {
  if (el) rouletteRefs.value[index] = el;
}

function startRoulette() {
  rouletteRefs.value.forEach(r => r?.startSpin());
  carouselRef.value?.next();
  statusButton.value = 2;
  setTimeout(() => statusButton.value = 1, 3000);
}

function changeActiveRoulette(count: number) {
  countRoulette.value = count;
  roulettes.value = Array.from({ length: count }, (_, i) => ({ id: `r${i + 1}` }));
}

// Кнопки выбора количества рулеток
const rouletteBtns = ref<HTMLElement[]>([]);
const borderStyle = ref({ transform: 'translateX(0)' });

const updateBorderPosition = () => {
  if (!rouletteBtns.value.length) return;
  const index = rouletteCountList.indexOf(countRoulette.value);
  if (index === -1) return;

  const btnWidth = rouletteBtns.value[0].offsetWidth;
  borderStyle.value.transform = `translateX(${index * btnWidth}px)`;
};

onMounted(async () => {
  await nextTick();
  rouletteBtns.value = Array.from(document.querySelectorAll('.home__buttons-item'));
  updateBorderPosition();
});

watch(countRoulette, updateBorderPosition);
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
      <div :class="['home__roulette-item', `layout-${countRoulette}`]">
        <Roulette
            v-for="(r, i) in roulettes"
            :key="r.id"
            :uid="r.id"
            :class="`roulette-size-${countRoulette}`"
            :ref="el => setRef(el, i)"
        />
      </div>
    </div>

    <div class="home__buttons container">
      <div class="home__buttons-border" :style="borderStyle"></div>
      <h3
          v-for="count in rouletteCountList"
          :key="count"
          @click="changeActiveRoulette(count)"
          :class="['home__buttons-item', { 'home__buttons-item-active': countRoulette === count }]"
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
  padding-bottom: 20rem;

  &__cards {
    display: flex;
    justify-content: center;
  }

  &__roulette {
    display: flex;
    justify-content: center;
    position: relative;
    width: 100%;
    height: 27.5rem;

    &-item {
      position: absolute;
      display: grid;
      gap: 1rem;
      justify-content: center;
      align-items: center;
      transition: all 0.5s ease;

      &.layout-1 { grid-template-columns: 1fr; }
      &.layout-3 { grid-template-columns: repeat(3, auto); padding-top: 5rem; }
      &.layout-5 { grid-template-columns: repeat(3, auto); grid-template-rows: repeat(2, auto); }
    }
  }

  &__buttons {
    position: relative;
    display: flex;
    justify-content: center;
    background-color: var(--color-black-opacity-40);
    border-radius: 2rem;
    padding: 0.5rem;

    &-item {
      display: flex;
      justify-content: center;
      align-items: center;
      width: 18.1rem;
      height: 8rem;
      font-size: 3.2rem;
      border-radius: 2rem;
      cursor: pointer;
      transition: font-size 0.3s ease;

      &-active { font-size: 4rem; }
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

  &__block { padding-top: 1.3rem; }

  &__spin {
    position: relative;
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    height: 10rem;
    border-radius: 2rem;
    background: linear-gradient(89.92deg, #01D9FF 0.06%, #0051FF 102.24%);
    transition: 0.5s ease;

    &-text { font-size: 3.2rem; transition: 0.5s ease; }

    &-image-left { width: 12.4rem; position: absolute; bottom: 0; left: 0; }
    &-image-right { width: 12.7rem; position: absolute; top: 0; right: 0; }
  }
}

/* Размеры рулеток */
.roulette-size-1 { width: 30rem; height: 27.5rem; transition: all 0.4s ease; }
.roulette-size-3 { width: 18.3rem; height: 17.3rem; transition: all 0.4s ease; }
.roulette-size-5 { width: 14.7rem; height: 14rem; transition: all 0.4s ease; }
</style>
