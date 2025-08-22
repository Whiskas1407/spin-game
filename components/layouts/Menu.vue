<script setup lang="ts">
import {useRoute} from "#vue-router";
const route = useRoute();

console.log(route)
</script>
<template>
  <div class="menu">
    <div class="menu__block">
      <NuxtLink to="/gifts" class="menu__item">
        <img class="menu__item-image" src="@/public/icons/gift.svg" alt="gift" />
        <p class="menu__item-text" :class="route.fullPath === '/gift' ? 'menu__item-text-active': ''">Мои подарки</p>
      </NuxtLink>
      <NuxtLink to="/" class="menu__item">
        <img class="menu__item-image" src="@/public/icons/noto_slot-machine.svg" alt="slot" />
        <p class="menu__item-text" :class="route.fullPath === '/' ? 'menu__item-text-active': ''">Главная</p>
      </NuxtLink>
      <NuxtLink to="/profile" class="menu__item">
        <img class="menu__item-image" src="@/public/icons/profile.svg" alt="gift" />
        <p class="menu__item-text" :class="route.fullPath === '/profile' ? 'menu__item-text-active': ''">Профиль</p>
      </NuxtLink>
    </div>
  </div>
</template>
<style lang="scss" scoped>
.menu {
  margin-top: 4rem;
  width: 100%;
  background-color: var(--color-black-opacity-50);
  z-index: 2;
  border-top: .1rem solid #FFFFFF;
  border-top-left-radius: 2rem;
  border-top-right-radius: 2rem;
  &__block {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 2rem 6.7rem 6rem;
  }
  &__item {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: .4rem;
    &-text {
      font-size: 1.4rem;
      color: var(--color-gray);
      &-active {
        background: linear-gradient(89.92deg, #01D9FF 0.06%, #0051FF 102.24%);
        background-clip: text;
        -webkit-text-fill-color: transparent;
      }
    }
    &-image {
      width: 4rem;
      height: 4rem;
    }
  }
}
</style>