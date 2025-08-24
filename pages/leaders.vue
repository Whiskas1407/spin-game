<script setup lang="ts">
import Header from "@/components/layouts/Header.vue";
</script>

<template>
  <div class="leaders">
    <Header hideUiButtons />

    <div class="leaders__info">
      <h3 class="leaders__title">Таблица лидеров</h3>
      <h5 class="leaders__subtitle">ТОП 100 ПОБЕДИТЕЛЕЙ</h5>

      <div class="leaders__items">
        <!-- Топ 3 -->
        <div class="leaders__item first">
          <h4 class="leaders__item-number">1#</h4>
          <img class="leaders__item-avatar" src="@/public/image/default-user.png" alt="user" />
          <h3 class="leaders__item-name">@username</h3>
          <div class="leaders__item-cup">
            <h4>500</h4>
            <img src="@/public/icons/white-cup.svg" alt="cup" />
          </div>
        </div>

        <div class="leaders__item second">
          <h4 class="leaders__item-number">2#</h4>
          <img class="leaders__item-avatar" src="@/public/image/default-user.png" alt="user" />
          <h3 class="leaders__item-name">@daun</h3>
          <div class="leaders__item-cup second">
            <h4>400</h4>
            <img src="@/public/icons/black-cup.svg" alt="cup" />
          </div>
        </div>

        <div class="leaders__item three">
          <h4 class="leaders__item-number">3#</h4>
          <img class="leaders__item-avatar" src="@/public/image/default-user.png" alt="user" />
          <h3 class="leaders__item-name">@chmo</h3>
          <div class="leaders__item-cup">
            <h4>200</h4>
            <img src="@/public/icons/white-cup.svg" alt="cup" />
          </div>
        </div>

        <!-- Другие -->
        <div class="leaders__other">
          <img src="/icons/left-line.svg" alt="line" />
          <h3>Другие</h3>
          <img src="/icons/right-line.svg" alt="line" />
        </div>

        <!-- Остальные топы -->
        <div v-for="i in 20" :key="i" class="leaders__item">
          <h4 class="leaders__item-number">{{ i + 3 }}#</h4>
          <img class="leaders__item-avatar" src="@/public/image/default-user.png" alt="user" />
          <h3 class="leaders__item-name">@chmo</h3>
          <div class="leaders__item-cup">
            <h4>200</h4>
            <img src="@/public/icons/white-cup.svg" alt="cup" />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.leaders {
  width: 100vw;
  height: 100%;
  padding: 10rem 0 20rem;
  background:
      url("@/public/image/leaders-bg-1.png") top right / 28.3rem no-repeat,
      url("@/public/image/leaders-bg-2.png") left center / 28.3rem no-repeat,
      url("@/public/image/leaders-bg-3.png") bottom right / 28.3rem no-repeat,
      linear-gradient(180deg, #F9B221 0%, #553900 100%);

  &__info {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  &__title {
    font-size: 3.2rem;
  }
  &__subtitle {
    color: var(--color-gray-white-dark);
  }

  &__items {
    padding-top: 2rem;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 2rem;
    max-height: calc(100vh - 36rem);
    overflow-y: scroll;
  }

  &__other {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 5rem;

    img {
      width: 19rem;
    }
  }

  &__item {
    display: flex;
    align-items: center;
    gap: 1rem;
    padding: 1rem;
    border-radius: 2rem;
    background-color: var(--color-white-opacity-30);
    min-width: 23rem;

    &-avatar {
      width: 3.5rem;
    }

    &-cup {
      display: flex;
      align-items: center;
      gap: 0.2rem;

      img {
        width: 2rem;
      }
    }
  }
}

/* Топ 3 стили */
.first {
  background-color: var(--color-gold);
  width: 27.5rem;
}

.second {
  background-color: var(--color-white);

  h3, h4 {
    color: var(--color-black);
  }
}

.three {
  background-color: var(--color-brown);
}
</style>