<script setup lang="ts">
import { ref } from "vue"
import Item1 from '~/public/image/items/item-1.png'
import Item2 from '~/public/image/items/item-2.png'
import Item3 from '~/public/image/items/item-3.png'

const items = [
  { id: 1, img: Item1, title: 'Lunar Snake', stars: '40 000 ⭐' },
  { id: 2, img: Item2, title: 'Plush Pepe', stars: '150 000 ⭐' },
  { id: 3, img: Item3, title: 'Cristal Stone', stars: '400 000 ⭐' },
  { id: 4, img: Item1, title: 'Lunar Snake', stars: '40 000 ⭐' },
  { id: 5, img: Item2, title: 'Plush Pepe', stars: '150 000 ⭐' },
]

const current = ref(0)

function next() {
  current.value = (current.value + 1) % items.length
}

defineExpose({ next })

</script>

<template>
  <div class="carousel">
    <div class="carousel__track">
      <div
          v-for="(item, index) in items"
          :key="item.id"
          class="carousel__item"
          :style="{
          transform: `translateX(${(index - current) * 120}%) scale(${index === current ? 1 : 0.8})`,
          opacity: Math.abs(index - current) > 1 ? 0 : 1,
          zIndex: index === current ? 2 : 1
        }"
      >
        <h2 class="carousel__title" :style="{ opacity: index === current ? 1 : 0 }">
          {{ item.title }}
        </h2>
        <h3 class="carousel__subtitle" :style="{ opacity: index === current ? 1 : 0 }">
          {{ item.stars }}
        </h3>
        <img :src="item.img" alt="image"/>
        <h5 class="carousel__bottom" :style="{ opacity: index === current ? 1 : 0 }">🍋🍋🍋 что бы получить</h5>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.carousel {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
  width: 100%;
  height: 39.5rem;

  &__track {
    display: flex;
    position: relative;
    width: 100%;
    height: 100%;
    top: -52.5%;
    left: -25%;
    z-index: 2;
  }

  &__item {
    position: absolute;
    top: 50%;
    left: 50%;
    width: 29rem;
    transform: translate(-50%, -50%);
    transition: transform 0.8s ease, opacity 0.8s ease;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 2rem;

    img {
      width: 100%;
      height: 100%;
      object-fit: contain;
      border-radius: 2rem;
    }
  }

  &__title, &__subtitle, &__bottom {
    transition: opacity 0.5s ease;
  }
  &__subtitle {
    position: absolute;
    top: 5.5rem;
    padding: .5rem 1rem;
    border-radius: 2rem;
    border: .1rem solid var(--color-light-blue);
    background-color: rgba(0, 0, 0, 0.5);
    backdrop-filter: blur(2rem);
  }
  &__bottom {
    position: absolute;
    bottom: -1.5rem;
    padding: .5rem 1rem;
    border-radius: 2rem;
    background-color: rgba(0, 0, 0, 0.25);
    backdrop-filter: blur(2rem);
  }
}

.btn {
  position: absolute;
  bottom: 2rem;
  background: rgba(0,0,0,0.3);
  color: white;
  border: none;
  padding: 1rem;
  cursor: pointer;
  z-index: 3;
}
</style>
