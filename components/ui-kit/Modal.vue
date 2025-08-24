<script setup>
import { ref } from 'vue';

const isVisible = ref(true);
const isHiding = ref(false);

defineProps({
  modelValue: Boolean,
  positionModal: String
})
const emit = defineEmits(['update:modelValue'])

function closeModal() {
  isHiding.value = true;
  setTimeout(() => {
    isVisible.value = false;
    isHiding.value = false;
    emit('update:modelValue', false)
  }, 300);
}

</script>
<template>
  <teleport to="body">
    <div
        v-if="modelValue"
        class="modal-overlay"
        :class="positionModal === 'bottom' ? 'modal-overlay-bottom' : ''"
        @click.self="closeModal"
    >
      <div :class="{ hide: isHiding }" class="modal-content" data-aos="zoom-in">
        <img
            class="modal-close"
            src="/icons/close.svg"
            alt="close"
            @click.self="closeModal"
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
    &-bottom {
      align-items: flex-end !important;
    }
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
    animation: showModal .4s ease;
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
    width: 100%;

    input {
      font-size: 6.4rem;
      border: none;
      background: none;
      width: 22rem;
      color: var(--color-white);
      outline: none;
      text-align: right;
    }
    img {
      width: 4rem;
    }
  }

  &__btn {
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 2rem !important;
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
    display: flex;
    align-items: center;
    gap: .5rem;
    img {
      width: 2.5rem;
    }
  }

  &__lang {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 1rem;
    padding: 2rem 0;
    width: 16rem;
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
        transition: .5s ease;
      }
    }
  }
}

.hide {
  animation: hideModal 0.3s forwards;
}

@keyframes showModal {
  0% {
    transform: translateY(100vh);
  }
  100% {
    transform: translateY(0);
  }
}

@keyframes hideModal {
  0% {
    transform: translateY(0);
  }
  100% {
    transform: translateY(100vh);
  }
}
</style>
