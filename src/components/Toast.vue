<template>
  <teleport to="#toast-container">
    <div v-if="isVisible" :class="`toast toast--${variant}`">
      <p class="toast__message">{{ msg }}</p>
      <button class="toast__close" @click="$emit('close')">
        <IconCloseToast />
      </button>
    </div>
  </teleport>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import IconCloseToast from './icons/IconCloseToast.vue';

export default defineComponent({
  components: { IconCloseToast },
  name: 'Toast',
  props: {
    msg: { type: String, required: true },
    variant: { type: String, default: 'success' },
    isVisible: { type: Boolean, default: false }
  }
});
</script>

<style lang="scss">
.toast {
  position: fixed;
  bottom: 30px;
  left: 50%;
  transform: translateX(-50%);

  padding: 8px 12px;
  box-shadow: 0px 2px 24px rgba(0, 0, 0, 0.08);
  border-radius: 6px;

  display: flex;
  align-items: center;
  width: fit-content;

  &--success {
    background: var(--c-toast-bg-success);
  }
}

.toast__message {
  margin: 0;
  color: var(--c-toast-text-primary);
  font-weight: var(--fw-medium);
  font-size: 14px;
  line-height: 20px;
}

.toast__close {
  @include reset-button;
  margin-left: 12px;
  cursor: pointer;
}
</style>