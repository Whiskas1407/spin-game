<script setup lang="ts">
import Header from "~/components/layouts/Header.vue";
import ButtonComponent from "~/components/ui-kit/ButtonComponent.vue";
</script>
<template>
  <div class="profile">
    <div class="container">
      <Header showSettings />
    </div>
    <div class="profile__block container">
      <div class="profile__info">
        <img src="@/public/image/default-user.png" alt="user" />
        <h3 class="profile__info-name">@ludoman</h3>
        <h5 class="profile__info-id">id: 123456789</h5>
      </div>
      <div class="profile__stats">
        <div class="profile__stats-item">
          <div class="profile__stats-item-info">
            <h3>69.4</h3>
            <img src="/icons/token.svg" alt="token" />
          </div>
          <h5>Оборот</h5>
        </div>
        <div class="profile__stats-item">
          <div class="profile__stats-item-info">
            <h3>5</h3>
            <img src="/icons/cup.svg" alt="cup" />
          </div>
          <h5>Побед</h5>
        </div>
        <div class="profile__stats-item">
          <div class="profile__stats-item-info">
            <h3>5</h3>
            <img src="/icons/gift-gradient.svg" alt="gift" />
          </div>
          <h5>Выведено</h5>
        </div>
      </div>
      <div class="profile__liders">
        <div class="profile__liders-info">
          <img src="@/public/image/gold-cup.png" alt="cup" />
          <h3>Таблица лидеров</h3>
        </div>
        <img class="profile__liders-arrow" src="/icons/arrow-right.svg" alt="arrow" />
      </div>
      <div class="profile__referral">
        <h3 class="profile__referral-title">💰Реферальная система</h3>
        <div class="profile__referral-item">
          <div class="profile__referral-item-info">
            <img src="/icons/flowbite_sale.svg" alt="flowbite" />
            <h3>Реферальные крутки</h3>
          </div>
          <h3 class="profile__referral-item-text">Получайте с депозита игрока 5%</h3>
        </div>
        <div class="profile__referral-item">
          <div class="profile__referral-item-info">
            <img src="/icons/flowbite_sale.svg" alt="flowbite" />
            <h3>Реферальные крутки</h3>
          </div>
          <h3 class="profile__referral-item-text">Получайте с депозита игрока 5%</h3>
        </div>
        <div class="profile__referral-item">
          <div class="profile__referral-item-info">
            <img src="/icons/flowbite_sale.svg" alt="flowbite" />
            <h3>Реферальные крутки</h3>
          </div>
          <h3 class="profile__referral-item-text">Получайте с депозита игрока 5%</h3>
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
    gap: .1rem;
    img {
      width: 20rem;
    }
    &-name {
      font-size: 3.2rem;
    }
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
      border-right: .1rem solid var(--color-white);
      width: 16.5rem;
      &-info {
        display: flex;
        align-items: center;
        gap: .5rem;
        h3 {
          font-size: 3.2rem;
        }
        img {
          width: 4rem;
        }
      }
      &:last-child {
        border: none
      }
    }
  }
  &__liders {
    margin-top: 2rem;
    display: flex;
    align-items: center;
    justify-content: space-between;
    width: 45.9rem;
    background: url("@/public/image/cup-bg.png"), linear-gradient(90deg, #281B00 0%, #F9B221 100%);
    padding: 1.5rem 2rem;
    border-radius: 2rem;
    background-repeat: no-repeat;
    background-position: bottom right;
    &-info {
      display: flex;
      align-items: center;
      gap: 1rem;
      h3 {
        font-size: 3.2rem;
      }
      img {
        width: 3.2rem;
      }
    }
    &-arrow {
      width: 1.5rem;
    }
  }
  &__referral {
    margin-top: 1rem;
    background-color: var(--color-black);
    border: .1rem solid var(--color-light-blue);
    width: 44.9rem;
    padding: 2rem;
    border-radius: 2rem;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 1rem;
    &-title {
      font-size: 3.2rem;
    }
    &-item {
      display: flex;
      flex-direction: column;
      align-items: flex-start;
      &-info {
        display: flex;
        align-items: center;
        gap: .5rem;
      }
      &-text {
        color: var(--color-gray);
      }
    }
    &-btn {
      padding: 2.3rem 5rem;
      border-radius: 2rem;
    }
  }
}
</style>