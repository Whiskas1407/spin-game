<script setup lang="ts">
import Header from "~/components/layouts/Header.vue";
import Modal from "~/components/ui-kit/Modal.vue";
import ButtonComponent from "~/components/ui-kit/ButtonComponent.vue";
import { ref, computed } from 'vue'

// --- Reactive state ---
const btnStatus = ref<number>(1)
const itemsList = ref<{ id: number }[]>([])
const showModalGifts = ref(false)
const gifts = Array.from({ length: 18 }, (_, i) => i + 1) // массив подарков 1..18

// --- Functions ---
const changeStatusButton = (id: number) => btnStatus.value = id

const setItem = (id: number) => {
  if (btnStatus.value !== 1) {
    const index = itemsList.value.findIndex(item => item.id === id)
    index >= 0 ? itemsList.value.splice(index, 1) : itemsList.value.push({ id })
  }
}

const isSelected = (id: number) => itemsList.value.some(item => item.id === id)

// --- Computed ---
const buttonText = computed(() => {
  if (btnStatus.value === 1) return 'Вывести подарки'
  if (btnStatus.value === 2 && itemsList.value.length === 0) return 'Выберите подарки'
  if (itemsList.value.length > 0) return `Вывести ${itemsList.value.length} подарок`
  return ''
})
</script>

<template>
  <div class="gift">
    <div class="container">
      <Header hideUiButtons />
    </div>

    <!-- Контент подарков -->
    <div class="gift__content">
      <h2 class="gift__content-text" v-if="gifts.length > 0">12 Подарков | 30.59 <span>TON</span></h2>
      <div class="gift__content-block" v-if="gifts.length > 0">
        <div
            v-for="id in gifts"
            :key="id"
            class="gift__content-item"
            @click="setItem(id)"
        >
          <img class="gift__content-image" src="@/public/image/items/item-1.png" alt="item" />
          <div :class="['gift__content-done', { 'gift__content-hidden': !isSelected(id) }]">
            <img src="@/public/icons/done-icon.svg" alt="done" />
          </div>
        </div>
      </div>
      <div class="gift__content-empty" v-else>
        <img src="@/public/icons/emoticon-cry.svg" alt="emoji" />
        <h3>Вы пока не выиграли <br> ни один подарок.</h3>
      </div>
    </div>

    <!-- Кнопка -->
    <div class="gift__block container">
      <button
          v-if="gifts.length > 0"
          class="gift__button"
          :class="{ 'gift__button-active': btnStatus !== 1 }"
          @click="btnStatus !== 1 && itemsList.length > 0 ? showModalGifts = true : changeStatusButton(2)"
      >
        <img v-if="btnStatus === 1" class="gift__button-image-center" src="@/public/icons/upload.svg" alt="upload" />
        <h3 class="gift__button-text">{{ buttonText }}</h3>
      </button>
      <NuxtLink to="/" v-else>
       <button  class="gift__button">
         <img class="gift__button-image-center" src="@/public/icons/noto_slot-machine.svg" alt="slot" />
         <h3 class="gift__button-text">Начать крутить...</h3>
       </button>
      </NuxtLink>
    </div>

    <!-- Модальное окно -->
    <Modal :model-value="showModalGifts" @update:modelValue="showModalGifts = $event">
      <div class="wallet">
        <div class="wallet__head">
          <h3>Вы уверены что хотите вывести <br> {{ itemsList.length }} подарок?</h3>
        </div>
        <div class="wallet__buttons">
          <ButtonComponent background="color-blue" class="wallet__btn" @click="showModalGifts = false">
            <h3>Да</h3>
          </ButtonComponent>
          <ButtonComponent background="color-black" class="wallet__btn" @click="showModalGifts = false">
            <h3>Нет</h3>
          </ButtonComponent>
        </div>
      </div>
    </Modal>
  </div>
</template>

<style lang="scss" scoped>
.gift {
  display: flex;
  flex-direction: column;
  padding-bottom: 20rem;

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
      border-radius: 2rem 2rem 0 0;
      background-color: var(--color-black-opacity-50);
      span {
        background: linear-gradient(89.92deg, #01D9FF 0.06%, #0051FF 102.24%);
        background-clip: text;
        -webkit-text-fill-color: transparent;
      }
    }

    &-block {
      max-height: 71rem;
      overflow-y: auto;
      background-color: var(--color-black-opacity-50);
      border: .1rem solid var(--color-light-blue);
      border-radius: 2rem;
      padding: 2rem;
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 2rem;
    }

    &-item { position: relative; }

    &-done {
      position: absolute;
      z-index: 2;
      top: 0; left: 0;
      width: 15rem; height: 15rem;
      display: flex; align-items: center; justify-content: center;
      background-color: var(--color-black-opacity-75);
      border: .4rem solid var(--color-light-blue);
      border-radius: 2rem;
      opacity: 1;
      transition: opacity 0.3s ease, transform 0.3s ease, visibility 0.3s;

      img { width: 3.9rem; }
    }

    &-hidden {
      opacity: 0;
      visibility: hidden;
      transition: opacity 0.3s ease, transform 0.3s ease, visibility 0.3s;
    }

    &-image { width: 15.5rem; height: 15.5rem; border: .1rem solid transparent; }
    &-empty {
      width: 100%;
      height: 60vh;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      gap: 1rem;
      img {
        width: 11.2rem;
      }
      h3 {
        text-align: center;
      }
    }
  }

  &__block { padding-top: 1.3rem; }

  &__button {
    position: relative;
    display: flex; gap: 1rem;
    align-items: center; justify-content: center;
    width: 100%;
    height: 10rem;
    border-radius: 2rem;
    background:
        url("@/public/image/tabler_gift-filled-left.png") left bottom / 10rem no-repeat,
        url("@/public/image/tabler_gift-filled-right.png") right bottom / 10rem no-repeat,
        linear-gradient(89.92deg, #01D9FF 0.06%, #0051FF 102.24%);
    border: .1rem solid transparent;

    &-active {
      background:
          url("@/public/image/tabler_gift-filled-right-a.png") left bottom / 10rem no-repeat,
          url("@/public/image/tabler_gift-filled-left-a.png") right bottom / 10rem no-repeat;
      border: .1rem solid var(--color-light-blue);
    }

    &-text { font-size: 3.2rem; }

    &-image-center { width: 2.5rem; }
  }
}
</style>
