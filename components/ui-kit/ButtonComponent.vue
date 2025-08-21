<script setup lang="ts">
const props = defineProps<{
  background?: string
}>()
</script>
<template>
  <button :style="{ background: props.background ? `var(--${props.background})` : `rgba(0, 0, 0, 0.3)` }">
    <slot />
  </button>
</template>
<style lang="scss" scoped>
button {
  padding: 1rem;
  display: flex;
  align-items: center;
  gap: 1rem;
  border-radius: 5rem;
  border-style: none;
}
</style>