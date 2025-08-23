<script setup lang="ts">
import Header from "~/components/layouts/Header.vue";

// Массив истории для рендера
const historyItems = Array.from({ length: 8 }, (_, i) => ({
  id: i + 1,
  username: '@username',
  userImage: '/image/default-user.png',
  itemImage: '/image/items/item-1.png',
  spins: 20,
  date: '21.08.2025 | 18:36'
}))
</script>

<template>
  <div class="history">
    <div class="container">
      <Header hideUiButtons />
    </div>

    <div class="history__content">
      <h2 class="history__title">ИСТОРИЯ КРУТОК</h2>

      <div class="history__list">
        <div v-for="item in historyItems" :key="item.id" class="history__item">
          <div class="history__user">
            <img :src="item.userImage" alt="user" />
            <h4>{{ item.username }}</h4>
          </div>

          <img class="history__item-image" :src="item.itemImage" alt="item" />

          <h5>c {{ item.spins }} крутки!</h5>
          <p>{{ item.date }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.history {
  background-image: url("/public/image/bg-history-1.png"), url("/public/image/bg-history-2.png");
  background-repeat: no-repeat;
  background-position: bottom left, top right;
  background-size: 70%;
  padding-bottom: 20rem;

  &__content {
    display: flex;
    flex-direction: column;
    align-items: center;

    & > .history__title {
      padding-top: 1rem;
      font-size: 3.2rem;
    }
  }

  &__list {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 4.5rem;
    padding-top: 2.5rem;
    height: 100vh;
    overflow-y: auto;
  }

  &__item {
    position: relative;
    display: flex;
    flex-direction: column;
    align-items: center;

    &-image {
      width: 20rem;
    }

    h5 {
      position: absolute;
      bottom: 2rem;
      padding: 0.5rem;
      border-radius: 1rem;
      background-color: var(--color-black-opacity-40);
    }

    p {
      padding-top: 2.2rem;
      font-size: 1.2rem;
    }
  }

  &__user {
    position: absolute;
    top: -1.8rem;
    display: flex;
    align-items: center;
    gap: 1rem;
    padding: 0.5rem;
    border-radius: 1rem;
    border-top: 0.1rem solid var(--color-light-blue);
    background-color: var(--color-black-opacity-50);

    img {
      width: 3rem;
    }
  }
}
</style>
