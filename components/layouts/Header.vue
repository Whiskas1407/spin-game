<script setup lang="ts">
import ButtonComponent from "~/components/ui-kit/ButtonComponent.vue";
import { ref, onMounted, onUnmounted } from 'vue'

const number = ref(494)
let intervalId: number | undefined

function randomChange() {
  const change = Math.random() < 0.5 ? -1 : 1
  number.value += change
}

onMounted(() => {
  intervalId = window.setInterval(randomChange, 3000)
})

onUnmounted(() => {
  if (intervalId) clearInterval(intervalId)
})
</script>
<template>
  <div class="header">
    <!--Кнопка-->
    <ButtonComponent>
      <img class="header__btn" src="@/public/icons/settings-rounded.svg" alt="icon" />
      <h5>История</h5>
    </ButtonComponent>
    <!--Онлайн-->
    <div class="header__online">
      <div class="header__online-status"></div>
      <h4>{{ number }} Онлайн</h4>
    </div>
    <!--Баланс-->
    <div class="header__balance">
      <ButtonComponent background="color-orange">
        <div class="header__balance-btn">
          <img class="header__balance-btn-image" src="@/public/icons/star-icon.svg" alt="star" />
          <h5>0</h5>
        </div>
        <img class="header__balance-btn-add" src="@/public/icons/add-white-icon.svg" alt="add-icon" />
      </ButtonComponent>
      <ButtonComponent background="color-blue">
        <div class="header__balance-btn">
          <img class="header__balance-btn-image" src="@/public/icons/token_ton.svg" alt="token" />
          <h5>0.00</h5>
        </div>
        <img class="header__balance-btn-add" src="@/public/icons/add-white-icon.svg" alt="add-icon" />
      </ButtonComponent>
    </div>
  </div>
</template>
<style lang="scss" scoped>
.header {
  padding-top: 2rem;
  display: flex;
  align-items: flex-start;
  justify-content: space-between;
  &__btn {
    width: 3rem;
  }
  &__online {
    display: flex;
    align-items: center;
    gap: 1rem;
    padding-top: 1.3rem;
    &-status {
      width: 1rem;
      height: 1rem;
      background-color: var(--color-green);
      border-radius: 100%;
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
      &-image {
        width: 2.4rem;
      }
      &-add {
        width: 3.3rem;
      }
    }
  }
}
</style>