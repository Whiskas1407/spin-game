<script setup lang="ts">
import HistoryBlock from "~/components/blocks/HistoryBlock.vue";
import Header from "~/components/layouts/Header.vue";
</script>
<template>
  <div class="gift">
    <div class="container">
      <Header hideUiButtons />
    </div>
    <div class="gift__content">
      <h2 class="gift__content-text">12 Подарков | 30.59 TON</h2>
      <div class="gift__content-block">
        <img class="gift__content-image" v-for="i of 18" :key="i" src="@/public/image/items/item-1.png" alt="item" />
      </div>
    </div>
    <div class="gift__block container">
      <div class="gift__button">
        <img class="gift__button-image-left" src="/image/tabler_gift-filled-left.png" alt="gift" />
        <img class="gift__button-image-center" src="/icons/upload.svg" alt="upload" />
        <h3 class="gift__button-text">Вывести подарки</h3>
        <img class="gift__button-image-right" src="/image/tabler_gift-filled-right.png" alt="gift" />
      </div>
    </div>
  </div>
</template>
<style lang="scss" scoped>
.gift {
  display: flex;
  flex-direction: column;
  &__content {
    padding-top: 3rem;
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    &-text {
      font-size: 3rem;
      padding: 1rem;
      border-top: .1rem solid var(--color-light-blue);
      border-top-left-radius: 2rem;
      border-top-right-radius: 2rem;
      background-color: var(--color-black-opacity-50);
    }
    &-block {
      max-height: 71rem;
      overflow-y: scroll;
      background-color: var(--color-black-opacity-50);
      border: .1rem solid var(--color-light-blue);
      border-radius: 2rem;
      padding: 2rem;
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      grid-gap: 2rem;
    }
    &-image {
      width: 15.5rem;
      height: 15.5rem;
    }
  }
  &__block {
    padding-top: 1.3rem;
  }
  &__button {
    position: relative;
    display: flex;
    gap: 1rem;
    align-items: center;
    justify-content: center;
    width: 100%;
    border-radius: 2rem;
    height: 10rem;
    background: linear-gradient(89.92deg, #01D9FF 0.06%, #0051FF 102.24%);
    &-text {
      font-size: 3.2rem;
    }
    &-image {
      &-left {
        width: 9rem;
        position: absolute;
        bottom: 0;
        left: 0;
      }
      &-center {
        width: 2.5rem;
      }
      &-right {
        width: 9rem;
        position: absolute;
        bottom: 0;
        right: 0;
      }
    }
  }
}
</style>