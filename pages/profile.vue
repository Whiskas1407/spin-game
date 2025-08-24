<script setup lang="ts">
import Header from "~/components/layouts/Header.vue";
import ButtonComponent from "~/components/ui-kit/ButtonComponent.vue";
const stats = [
  { value: 69.4, label: 'Оборот', icon: '/icons/token.svg' },
  { value: 5, label: 'Побед', icon: '/icons/cup.svg' },
  { value: 5, label: 'Выведено', icon: '/icons/gift-gradient.svg' }
]

const referrals = Array(3).fill({ text: 'Получайте с депозита игрока 5%' })
</script>


<template>
  <div class="profile">
    <div class="container">
      <Header showSettings />
    </div>

    <div class="profile__block container">
      <!-- Info -->
      <div class="profile__info">
        <img src="@/public/image/default-user.png" alt="user" />
        <h3 class="profile__info-name">@ludoman</h3>
        <h5 class="profile__info-id">id: 123456789</h5>
      </div>

      <!-- Stats -->
      <div class="profile__stats">
        <div class="profile__stats-item" v-for="(stat, index) in stats" :key="index">
          <div class="profile__stats-item-info">
            <h3 class="font-light">{{ stat.value }}</h3>
            <img :src="stat.icon" :alt="stat.label" />
          </div>
          <h5 class="font-light">{{ stat.label }}</h5>
        </div>
      </div>

      <!-- Leaders -->
      <NuxtLink to="/leaders" class="profile__liders">
        <div class="profile__liders-info">
          <img src="@/public/image/gold-cup.png" alt="cup" />
          <h3>Таблица лидеров</h3>
        </div>
        <img class="profile__liders-arrow" src="/icons/arrow-right.svg" alt="arrow" />
      </NuxtLink>

      <!-- Referral -->
      <div class="profile__referral">
        <h3 class="profile__referral-title">
          <img src="@/public/image/money.png" alt="money" />
          Реферальная система
        </h3>
        <div
            class="profile__referral-item"
            v-for="(ref, index) in referrals"
            :key="index"
        >
          <div class="profile__referral-item-info">
            <img src="/icons/flowbite_sale.svg" alt="flowbite" />
            <h3>Реферальные крутки</h3>
          </div>
          <h3 class="profile__referral-item-text">{{ ref.text }}</h3>
        </div>
        <ButtonComponent background="color-gradient-blue" class="profile__referral-btn">
          <img src="/icons/copy.svg" alt="copy" />
          <h3>Скопировать ссылку</h3>
        </ButtonComponent>
      </div>
    </div>
  </div>
</template>
<style lang="scss" scoped>
.profile {
  padding-bottom: 20rem;

  &__block {
    position: relative;
    top: -4rem;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  &__info {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 0.1rem;

    img { width: 20rem; }

    &-name { font-size: 3.2rem; }

    &-id {
      background: linear-gradient(0deg, #45C6EA -39.29%, #57CDED 22.73%, #2A31B5 138.95%);
      background-clip: text;
      -webkit-text-fill-color: transparent;
    }
  }

  &__stats {
    display: flex;
    align-items: center;
    justify-content: center;
    padding-top: 2rem;

    &-item {
      display: flex;
      flex-direction: column;
      align-items: center;
      width: 16.5rem;
      border-right: 0.1rem solid var(--color-white);

      &:last-child { border: none; }

      &-info {
        display: flex;
        align-items: center;
        gap: 0.5rem;

        h3 { font-size: 3.2rem; }
        img { width: 4rem; }
      }
    }
  }

  &__liders {
    margin-top: 2rem;
    display: flex;
    align-items: center;
    justify-content: space-between;
    width: 45.9rem;
    padding: 1.5rem 2rem;
    border-radius: 2rem;
    background: url("@/public/image/cup-bg.png") no-repeat bottom right,
    linear-gradient(90deg, #281B00 0%, #F9B221 100%);

    &-info {
      display: flex;
      align-items: center;
      gap: 1rem;

      h3 { font-size: 3.2rem; }
      img { width: 3.2rem; }
    }

    &-arrow { width: 1.5rem; }
  }

  &__referral {
    margin-top: 1rem;
    width: 44.9rem;
    padding: 2rem;
    border-radius: 2rem;
    background-color: var(--color-black);
    border: 0.1rem solid var(--color-light-blue);
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 1rem;

    &-title {
      display: flex;
      align-items: center;
      justify-content: flex-start;
      font-size: 3.2rem;
      gap: 1rem;
      img {
        width: 3rem;
      }
    }

    &-item {
      display: flex;
      flex-direction: column;
      align-items: flex-start;

      &-info {
        display: flex;
        align-items: center;
        gap: 0.5rem;
      }

      &-text { color: var(--color-gray); }
    }

    &-btn { padding: 2.3rem 5rem; border-radius: 2rem; }
  }
}
</style>
