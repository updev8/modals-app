<template>
  <div class="toast">
    <p class="toast__message">Спасибо, отзыв опубликован!</p>
    <button class="toast__close"><IconCloseToast /></button>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import IconCloseToast from './icons/IconCloseToast.vue';

export default defineComponent({
  components: { IconCloseToast },
  name: 'Toast'
});
</script>

<style lang="scss">
.toast {
  padding: 8px 12px;
  background: var(--c-toast-bg-success);
  box-shadow: 0px 2px 24px rgba(0, 0, 0, 0.08);
  border-radius: 6px;
  display: flex;
  align-items: center;
  width: fit-content;

  position: absolute;
  bottom: 30px;
  left: 50%;
  transform: translateX(-50%);
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