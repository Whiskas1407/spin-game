<script setup lang="ts">
interface Item {
  id: number;
  active: boolean;
}

defineProps<{
  item: Item;
}>();
</script>

<template>
  <div :class="['case', { active: item.active }]">
    <img class="case__image" src="/image/default-user.png" alt="user" />
    <div class="case__user">
      <p class="case__user-name font-semibold">@username</p>
      <p class="font-semibold">{{ item.active ? 'Выбил' : 'Не выбил' }}</p>
    </div>
    <img class="case__roulette" src="/image/ruletka-icon.png" alt="ruletka" />
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
      opacity: 0.2;
    }
  }

  &__roulette {
    width: 3.2rem;
  }
}

.active {
  background: var(--color-green-opacity-30);
}
</style>