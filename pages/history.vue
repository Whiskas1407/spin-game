<script setup lang="ts">
import HistoryBlock from "~/components/blocks/HistoryBlock.vue";
import Header from "~/components/layouts/Header.vue";
</script>
<template>
  <div class="history">
    <div class="container">
      <Header hideUiButtons />
    </div>
    <div class="history__content">
      <h2 class="history__content-title">ИСТОРИЯ КРУТОК</h2>
      <div class="history__content-block">
        <div v-for="i of 5" :key="i" class="history__content-item">
          <div class="history__content-item-user">
            <img src="@/public/image/default-user.png" alt="user" />
            <h4>@username</h4>
          </div>
          <img class="history__content-item-image" src="@/public/image/items/item-1.png" alt="item" />
          <h5>c 20 крутки!</h5>
          <p>21.08.2025 | 18:36</p>
        </div>
      </div>
    </div>
  </div>
</template>
<style lang="scss" scoped>
.history {
  background-image: url("/public/image/bg-history-1.png"), url("/public/image/bg-history-2.png");
  background-repeat: no-repeat, no-repeat;
  background-position: bottom left, top right;
  background-size: 70%;
  &__content {
    display: flex;
    align-items: center;
    flex-direction: column;
    &-title {
      padding-top: 1rem;
      font-size: 3.2rem;
    }
    &-block {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 4.5rem;
      padding-top: 2.5rem;
      height: 100vh;
      overflow-y: scroll;
    }
    &-item {
      position: relative;
      display: flex;
      flex-direction: column;
      align-items: center;
      &-user {
        position: absolute;
        top: -1.8rem;
        display: flex;
        align-items: center;
        gap: 1rem;
        padding: .5rem;
        border-radius: 1rem;
        border-top: .1rem solid var(--color-light-blue);
        background-color: var(--color-black-opacity-50);
        img {
          width: 3rem;
        }
      }
      &-image {
        width: 20rem;
      }
      h5 {
        position: absolute;
        bottom: 2rem;
        padding: .5rem;
        border-radius: 1rem;
        background-color: var(--color-black-opacity-40);
      }
      p {
        padding-top: 2.2rem;
        font-size: 1.2rem;
      }
    }
  }
}
</style>