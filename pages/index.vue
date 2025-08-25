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

const mainRoulette = { id: 'main' }; // Центральная рулетка
const roulettes = ref([{ ...mainRoulette }]);

function changeActiveRoulette(count: number) {
  countRoulette.value = count;

  if (count === 1) {
    roulettes.value = [{ ...mainRoulette }];
  } else if (count === 3) {
    roulettes.value = [
      { id: 'left' },
      { ...mainRoulette },
      { id: 'right' }
    ];
  } else if (count === 5) {
    roulettes.value = [
      { id: 'left1' },
      { id: 'left2' },
      { ...mainRoulette },
      { id: 'right1' },
      { id: 'right2' }
    ];
  }
}


// Кнопки выбора количества рулеток
const rouletteBtns = ref<HTMLElement[]>([]);
const borderStyle = ref({ transform: 'translateX(0)' });

const updateBorderPosition = () => {
  if (!rouletteBtns.value.length) return;

  const index = rouletteCountList.indexOf(countRoulette.value);
  if (index === -1) return;

  const activeBtn = rouletteBtns.value[index];
  const parent = activeBtn.parentElement as HTMLElement;

  if (!parent) return;

  const parentRect = parent.getBoundingClientRect();
  const btnRect = activeBtn.getBoundingClientRect();

  const offsetX = btnRect.left - parentRect.left;

  borderStyle.value = {
    transform: `translateX(${offsetX}px)`,
    width: `${btnRect.width}px` // динамически подстраиваем ширину
  };
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
      <button class="home__spin" @click="startRoulette">
        <h3 class="home__spin-text font-medium" v-show="statusButton === 1">КРУТИТЬ за 40 <img src="/icons/Star.svg" alt="star" /></h3>
        <h3 class="home__spin-text font-medium" v-show="statusButton === 2">Крутим...</h3>
      </button>
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
    }
  }

  &__buttons {
    position: relative;
    display: flex;
    justify-content: space-between;
    background-color: var(--color-black-opacity-40);
    border-radius: 2rem;
    padding: 0.5rem;
    margin: 5rem 2rem 1rem;
    @media (min-width: 768px) {
      margin-top: 7rem;
    }

    &-item {
      display: flex;
      justify-content: center;
      align-items: center;
      width: 22rem;
      height: 8rem;
      font-size: 3.2rem;
      border-radius: 2rem;
      cursor: pointer;
      transition: font-size 0.3s ease;

      &-active { font-size: 4rem; }
    }
    &-border {
      position: absolute;
      top: 0;
      left: 0;
      height: 100%; /* чтобы всегда по высоте кнопки */
      border-radius: 2rem;
      border: 0.2rem solid var(--color-light-blue);
      transition: transform 0.3s ease, width 0.3s ease; /* анимируем и позицию, и ширину */
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
    background:
        url("@/public/image/spin-image-1.png") bottom left / 13rem no-repeat,
        url("@/public/image/spin-image-2.png") top right / 13rem no-repeat,
        linear-gradient(89.92deg, #01D9FF 0.06%, #0051FF 102.24%);
    transition: 0.5s ease;

    &-text {
      font-size: 3.2rem;
      transition: 0.5s ease;
      display: flex;
      align-items: center;
      gap: .5rem;
      img {
        width: 3.5rem;
      }
    }

    &-image-left { width: 12.4rem; position: absolute; bottom: 0; left: 0; }
    &-image-right { width: 12.7rem; position: absolute; top: 0; right: 0; }
  }
}

.home__roulette-item.layout-5 {
  display: flex;
  flex-wrap: wrap;        /* перенос элементов на новую строку */
  justify-content: center; /* центрируем все по горизонтали */
  column-gap: 6rem; /* расстояние между рулетками */
  @media (min-width: 768px) {
    column-gap: 12rem;
  }
}

.home__roulette-item.layout-5 > :nth-child(1),
.home__roulette-item.layout-5 > :nth-child(2),
.home__roulette-item.layout-5 > :nth-child(3) {
  flex: 0 0 auto; /* верхние 3 рулетки занимают своё место */
}

.home__roulette-item.layout-5 > :nth-child(4),
.home__roulette-item.layout-5 > :nth-child(5) {
  flex: 0 0 auto;
}



/* Размеры рулеток */
.roulette-size-1 { width: 30rem; height: 27.5rem; transition: all 0.4s ease; }
.roulette-size-3 { width: 18.3rem; height: 17.3rem; transition: all 0.4s ease; }
.roulette-size-5 {
  width: 16.3rem;
  height: 16rem;
  transition: all 0.4s ease;
}
</style>
