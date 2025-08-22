<script setup lang="ts">
import Header from "~/components/layouts/Header.vue";
import { ref } from 'vue'

const btnStatus = ref<number>(1)
const itemsList = ref<{ id: number }[]>([])

function changeStatusButton(id: number) {
  btnStatus.value = id
}

function setItem(id: number) {
  if( btnStatus.value !== 1 ) {
    const exists = itemsList.value.some(item => item.id === id)
    if (exists) {
      itemsList.value = itemsList.value.filter(item => item.id !== id)
    } else {
      itemsList.value.push({ id })
    }
  }
}

function isSelected(id: number) {
  return itemsList.value.some(item => item.id === id)
}
</script>

<template>
  <div class="gift">
    <div class="container">
      <Header hideUiButtons />
    </div>
    <div class="gift__content">
      <h2 class="gift__content-text">12 Подарков | 30.59 TON</h2>
      <div class="gift__content-block">
        <div
            class="gift__content-item"
            v-for="i in 18"
            :key="i"
            @click="setItem(i)"
        >
          <img class="gift__content-image" src="@/public/image/items/item-1.png" alt="item" />
          <div :class="['gift__content-done', { 'gift__content-hidden': !isSelected(i) }]">
            <img src="/icons/done-icon.svg" alt="done" />
          </div>
        </div>
      </div>
    </div>
    <div class="gift__block container">
      <div class="gift__button" :class="{ 'gift__button-active': btnStatus !== 1 }" @click="changeStatusButton(2)">
        <img v-if="btnStatus === 1" class="gift__button-image-center" src="/icons/upload.svg" alt="upload" />
        <h3 class="gift__button-text" v-if="btnStatus === 1">Вывести подарки</h3>
        <h3 class="gift__button-text" v-if="btnStatus === 2 && itemsList.length === 0">Выберите подарки</h3>
        <h3 class="gift__button-text" v-if="itemsList.length > 0">Вывести {{ itemsList.length }} подарок</h3>
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
    &-item {
      position: relative;
    }
    &-done {
      position: absolute;
      z-index: 2;
      background-color: var(--color-black-opacity-75);
      left: 0;
      top: 0;
      display: flex;
      align-items: center;
      justify-content: center;
      width: 15rem;
      height: 15rem;
      border: .4rem solid var(--color-light-blue);
      border-radius: 2rem;
      img {
        width: 3.9rem;
      }
    }
    &-hidden {
      display: none;
    }
    &-image {
      width: 15.5rem;
      height: 15.5rem;
      border: .1rem solid transparent;
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
    background:
        url("@/public/image/tabler_gift-filled-left.png") left bottom no-repeat,
        url("@/public/image/tabler_gift-filled-right.png") right bottom no-repeat,
        linear-gradient(89.92deg, #01D9FF 0.06%, #0051FF 102.24%);
    border: .1rem solid transparent;
    &-active {
      background:
          url("@/public/image/tabler_gift-filled-right-a.png") left bottom no-repeat,
          url("@/public/image/tabler_gift-filled-left-a.png") right bottom no-repeat;
      border: .1rem solid var(--color-light-blue);
    }
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