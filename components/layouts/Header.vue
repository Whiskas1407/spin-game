<script setup lang="ts">
import ButtonComponent from "~/components/ui-kit/ButtonComponent.vue";
import Modal from "~/components/ui-kit/Modal.vue";
import { ref, onMounted, onUnmounted } from 'vue'

defineProps<{
  hideUiButtons?: boolean
  showSettings?: boolean
}>()

const number = ref(494)               // Онлайн
const address = ref<string | boolean>(false)
const balance = ref(59.39)
const changeCountStars = ref(false)
const activeLang = ref(1)
const statusModal = ref({
  buyToken: false,
  buyStar: false,
  settings: false
})
let intervalId: number | undefined
let countTon = ref(0)
function connectWallet() {
  address.value = 'UQDhAD.....yWYo7B'
}

function randomChange() {
  number.value += Math.random() < 0.5 ? -1 : 1
}

onMounted(() => {
  intervalId = window.setInterval(randomChange, 3000)
})

onUnmounted(() => {
  intervalId && clearInterval(intervalId)
})

const languages = [
  { id: 1, label: 'English', img: '/image/lang-us.png' },
  { id: 2, label: 'Русский', img: '/image/lang-ru.png' },
  { id: 3, label: 'Chinese', img: '/image/lang-ch.png' }
]
</script>

<template>
  <div class="header" :class="{ 'hide-button': hideUiButtons }">
    <!-- Кнопки -->
    <NuxtLink v-if="!hideUiButtons && !showSettings" to="/history">
      <ButtonComponent>
        <img class="header__btn" src="@/public/icons/settings-rounded.svg" alt="icon" />
        <h5>История</h5>
      </ButtonComponent>
    </NuxtLink>

    <ButtonComponent
        v-if="!hideUiButtons && showSettings"
        @click="statusModal.settings = true"
    >
      <img class="header__btn" src="@/public/icons/settings.svg" alt="icon" />
      <h5>Настройки</h5>
    </ButtonComponent>

    <!-- Онлайн -->
    <div class="header__online">
      <div class="header__online-status"></div>
      <h4 class="font-light">{{ number }} Онлайн</h4>
    </div>

    <!-- Баланс -->
    <div class="header__balance" v-if="!hideUiButtons">
      <ButtonComponent background="color-orange" @click="statusModal.buyStar = true">
        <div class="header__balance-btn">
          <img class="header__balance-btn-image" src="@/public/icons/star-icon.svg" alt="star" />
          <h5 class="font-medium">0</h5>
        </div>
        <img class="header__balance-btn-add" src="@/public/icons/add-white-icon.svg" alt="add-icon" />
      </ButtonComponent>

      <ButtonComponent background="color-blue" @click="statusModal.buyToken = true">
        <div class="header__balance-btn">
          <img class="header__balance-btn-image" src="@/public/icons/token_ton.svg" alt="token" />
          <h5 class="font-medium">0.00</h5>
        </div>
        <img class="header__balance-btn-add" src="@/public/icons/add-white-icon.svg" alt="add-icon" />
      </ButtonComponent>
    </div>

    <!-- Модалки -->
    <!-- Пополнение TON -->
    <Modal :model-value="statusModal.buyToken" @update:modelValue="statusModal.buyToken = $event">
      <div class="wallet">
        <div class="wallet__head">
          <div class="wallet__head-block">
            <h4 class="wallet__head-address">{{ address ? address : 'Кошелёк не подключён' }}</h4>
            <div class="wallet__head-connect" @click="connectWallet">
              <h4>{{ address ? `${balance} TON` : 'Connect' }}</h4>
              <img v-if="!address" src="/icons/blue-plus.svg" alt="plus" />
            </div>
          </div>
          <div class="wallet__head-text">
            <h2>Пополнение TON</h2>
            <h4>Выберите кол-во TON</h4>
          </div>
        </div>
        <div class="wallet__input">
          <input v-model="countTon" type="number" />
          <h2 class="wallet__input-text">
            {{ countTon ? countTon : 0 }} TON
          </h2>
        </div>
        <ButtonComponent :background="address ? 'color-blue' : 'color-gray-opacity-10'" class="wallet__btn">
          <h3>{{ address ? 'Пополнить' : 'Сперва - подключите кошелёк' }}</h3>
        </ButtonComponent>
      </div>
    </Modal>

    <!-- Пополнение звезд -->
    <Modal :model-value="statusModal.buyStar" @update:modelValue="statusModal.buyStar = $event">
      <div class="wallet">
        <div class="wallet__head">
          <div class="wallet__head-text">
            <h2>Пополнение звёзд</h2>
            <h4>{{ !changeCountStars ? 'Пополняй или пиздуй нахуй' : 'Выберите точную сумму'}}</h4>
          </div>
        </div>

        <div class="wallet__items" v-if="!changeCountStars">
          <h3 class="wallet__item" v-for="amount in [100, 300, 500]" :key="amount">{{ amount }} <img src="@/public/icons/Star.svg" alt="icon" /></h3>
        </div>

        <div class="wallet__input" v-else>
          <input v-model="countTon" type="number" />
          <h2 class="wallet__input-text">
            {{ countTon ? countTon : 0 }}
            <img src="@/public/icons/Star.svg" alt="icon" />
          </h2>
        </div>

        <ButtonComponent
            v-if="!changeCountStars"
            background="color-blue"
            class="wallet__btn"
            @click="changeCountStars = true"
        >
          <h3>Выбрать свою сумму</h3>
        </ButtonComponent>

        <div class="wallet__buttons" v-else>
          <ButtonComponent background="color-blue" class="wallet__btn">
            <h3>Отправить</h3>
          </ButtonComponent>
          <ButtonComponent background="color-black" class="wallet__btn" @click="changeCountStars = false">
            <h3>Назад</h3>
          </ButtonComponent>
        </div>
      </div>
    </Modal>

    <!-- Настройки -->
    <Modal :model-value="statusModal.settings" @update:modelValue="statusModal.settings = $event" positionModal="bottom">
      <div class="wallet">
        <div class="wallet__head">
          <div class="wallet__head-text">
            <h2>Настройки</h2>
            <h4>Язык</h4>
          </div>
          <div class="wallet__items">
            <div
                v-for="lang in languages"
                :key="lang.id"
                class="wallet__lang"
                :class="{ 'wallet__lang-active-bg': activeLang === lang.id }"
                @click="activeLang = lang.id"
            >
              <img class="wallet__lang-img" :src="lang.img" :alt="lang.label" />
              <h3 :class="{ 'wallet__lang-active': activeLang === lang.id }">{{ lang.label }}</h3>
            </div>
          </div>

          <ButtonComponent background="color-gradient-blue" class="wallet__btn-l" @click="statusModal.settings = false">
            <img src="@/public/icons/support.svg" alt="support">
            <h3>Написать в поддержку</h3>
          </ButtonComponent>
        </div>
      </div>
    </Modal>
  </div>
</template>

<style lang="scss" scoped>
.header {
  position: relative;
  z-index: 10;
  padding-top: 2rem;
  display: flex;
  align-items: flex-start;
  justify-content: space-between;

  &__btn { width: 3rem; }

  &__online {
    display: flex;
    align-items: center;
    gap: 1rem;
    padding-top: 1.3rem;

    &-status {
      width: 1rem;
      height: 1rem;
      background-color: var(--color-green);
      border-radius: 50%;
    }
  }

  &__balance {
    display: flex;
    flex-direction: column;
    align-items: flex-end;
    gap: 1rem;

    &-btn {
      display: flex;
      align-items: center;
      gap: 0.2rem;

      &-image { width: 2.4rem; }
      &-add { width: 3.3rem; }
    }
  }
}

.hide-button { justify-content: center; }
</style>
