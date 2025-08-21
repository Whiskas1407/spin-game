<script setup lang="ts">

</script>
<template>
  <div class="case">
    <img class="case__image" src="/image/default-user.png" alt="user" />
    <div class="case__user">
      <p class="case__user-name font-medium">@username</p>
      <p class="font-medium">Не выбил</p>
    </div>
    <img class="case__roulette" src="/image/ruletka-icon.png" alt="ruletka">
  </div>
</template>
<style lang="scss" scoped>
.case {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 1rem 1.5rem;
  background: var(--color-black-opacity-50);
  border-radius: 2rem;
  gap: 1rem;
  &__image {
    width: 5rem;
  }
  &__user {
    display: flex;
    flex-direction: column;
    gap: 1rem;
    align-items: flex-start;
    &-name {
      opacity: .2;
    }
  }
  &__roulette {
    width: 3.2rem;
  }
}
</style>