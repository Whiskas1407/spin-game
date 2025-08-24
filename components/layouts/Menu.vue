<script setup lang="ts">
import { useRoute } from '#vue-router';
import { computed } from 'vue';

const route = useRoute();

const menuItems = [
  {
    path: '/gifts',
    label: 'Мои подарки',
    icon: {
      default: '/icons/gift.svg',
      active: '/icons/gift-gold.svg'
    }
  },
  {
    path: '/',
    label: 'Главная',
    icon: {
      default: '/icons/noto_slot-machine.svg',
      active: '/icons/slot-gold.svg'
    }
  },
  {
    path: '/profile',
    label: 'Профиль',
    icon: {
      default: '/icons/profile.svg',
      active: '/icons/profile-gold.svg'
    }
  }
];

const isLeaders = computed(() => route.fullPath === '/leaders');

const getIcon = (item: any) =>
    isLeaders.value ? item.icon.active : item.icon.default;

const isActiveText = (path: string) => route.fullPath === path;
const isGoldProfileText = (path: string) =>
    isLeaders.value && path === '/profile';
</script>

<template>
  <div class="menu">
    <div class="menu__block">
      <NuxtLink
          v-for="item in menuItems"
          :key="item.path"
          :to="item.path"
          class="menu__item"
      >
        <img
            class="menu__item-image"
            :src="getIcon(item)"
            :alt="item.label"
        />
        <p
            class="menu__item-text"
            :class="{
            'menu__item-text-active': isActiveText(item.path),
            'menu__item-text-active-gold': isGoldProfileText(item.path)
          }"
        >
          {{ item.label }}
        </p>
      </NuxtLink>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.menu {
  position: fixed;
  bottom: -.1rem;
  width: 100%;
  background-color: var(--color-black-opacity-50);
  z-index: 2;
  border-top: .1rem solid #FFFFFF;
  border-top-left-radius: 2rem;
  border-top-right-radius: 2rem;
  backdrop-filter: blur(50rem);

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

      &-active-gold {
        background: linear-gradient(89.92deg, #F9B221 0.06%, #B58013 102.24%);
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

@media screen and (min-width: 768px) {
  .menu {
    max-width: 769px;
  }
}
</style>
