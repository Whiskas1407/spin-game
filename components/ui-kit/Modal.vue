<script setup>
defineProps({ modelValue: Boolean })
const emit = defineEmits(['update:modelValue'])
</script>
<template>
  <teleport to="body">
    <div
        v-if="modelValue"
        class="modal-overlay"
        @click.self="emit('update:modelValue', false)"
    >
      <div class="modal-content" data-aos="zoom-in">
        <img
            class="modal-close"
            src="/icons/close.svg"
            alt="close"
            @click.self="emit('update:modelValue', false)"
        />
        <slot />
      </div>
    </div>
  </teleport>
</template>
<style lang="scss">
/* --- Общие стили модалки --- */
.modal {
  &-overlay {
    position: fixed;
    inset: 0;
    z-index: 10;
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: rgba(0, 0, 0, 0.6);
  }

  &-content {
    position: relative;
    background-color: var(--color-gray-opacity-20);
    backdrop-filter: blur(2rem);
    border-radius: 2rem;
    padding: 5rem 3rem;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 4rem;
    max-width: 100%;
    max-height: 90vh;
    overflow-y: auto;
  }

  &-close {
    position: absolute;
    top: 1rem;
    right: 2rem;
    width: 5rem;
    cursor: pointer;
  }
}

/* --- Контент модалки (Wallet) --- */
.wallet {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 4rem;

  &__head {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 2rem;

    &-block {
      display: flex;
      align-items: center;
      gap: 2rem;
    }

    &-address {
      color: var(--color-gray-light);
      padding-right: 1rem;
      border-right: 0.1rem solid var(--color-gray-light);
    }

    &-connect {
      display: flex;
      align-items: center;
      gap: 1rem;

      img {
        width: 2.4rem;
      }

      h4 {
        color: var(--color-blue);
      }
    }

    &-text {
      h2 {
        font-size: 3.6rem;
        text-align: center;
      }
      h4 {
        color: var(--color-gray-light);
        text-align: center;
      }
    }

    h3 {
      text-align: center;
    }
  }

  &__input {
    display: flex;
    align-items: center;
    gap: 1rem;

    input {
      font-size: 6.4rem;
      border: none;
      background: none;
      width: 7rem;
      max-width: 12rem;
      color: var(--color-white);
      outline: none;
    }

    h2 {
      font-size: 3.6rem;
      color: var(--color-gray-light);
    }
  }

  &__btn {
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 2rem;
    height: 8rem;

    &,
    &-l {
      gap: 1rem;
    }

    & {
      width: 48rem;
    }

    &-l {
      display: flex;
      align-items: center;
      justify-content: center;
      border-radius: 2rem !important;
      width: 51.6rem !important;
      height: 8rem !important;

      img {
        width: 3.2rem;
      }
    }
  }

  &__buttons {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 1rem;
  }

  &__items {
    display: flex;
    align-items: center;
    gap: 1rem;
  }

  &__item {
    padding: 2rem 3rem;
    border: 0.2rem solid var(--color-orange);
    background-color: var(--color-black);
    border-radius: 2rem;
  }

  &__lang {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 1rem;
    padding: 2rem;
    border-radius: 2rem;
    border: 0.2rem solid var(--color-blue);

    &-img {
      width: 3.2rem;
    }

    &-active {
      color: var(--color-black);

      &-bg {
        background-color: var(--color-white);
        border: 0.2rem solid transparent;
      }
    }
  }
}
</style>
