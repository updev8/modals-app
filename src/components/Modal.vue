<template>
  <div v-if="show" class="modal">
    <div class="modal__window">
      <div class="modal__header">
        <button class="modal__back">
          <IconBack />
        </button>
        <h2 class="modal__title">{{ title }}</h2>
        <button class="modal__close" @click="$emit('close')">
          <IconClose />
        </button>
      </div>
      <div class="modal__content">
        <slot />
      </div>
      <div class="modal__footer">
        <button class="modal__button button">
          {{ buttonText }}
        </button>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import IconBack from './icons/IconBack.vue';
import IconClose from './icons/IconClose.vue';

export default defineComponent({
  name: 'Modal',
  components: { IconClose, IconBack },
  props: {
    show: Boolean,
    title: String,
    buttonText: String
  }
});
</script>

<style lang="scss">
.modal {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  z-index: 100;
  background: var(--c-bg-overlay-primary);

  display: flex;
  justify-content: center;

  padding-top: 8.9vh;
}

.modal__window {
  --modal-window-padding: 16px;
  --modal-window-padding-negative: calc(var(--modal-window-padding) * -1);

  margin-top: auto;
  background: var(--c-bg-modal-primary);
  border-radius: 16px 16px 0px 0px;
  padding: var(--modal-window-padding);
  min-width: 320px;
  max-width: 650px;
  position: relative;

  &::before {
    content: '';
    position: absolute;
    top: 4px;
    left: 50%;
    transform: translateX(-50%);

    width: 24px;
    height: 3px;

    border-radius: 4px;
    background: var(--c-accent-bg-tertiary);
  }
}

.modal__header {
  padding-bottom: var(--modal-window-padding);
  display: flex;
  align-items: center;
}

.modal__back {
  display: none;
  @include reset-button;

  &--visible {
    display: inline-block;
  }
}

.modal__title {
  margin: 0;
  font-weight: var(--fw-semi-bold);
  font-size: 16px;
  line-height: 24px;
}

.modal__close {
  @include reset-button;
  margin-left: auto;
  padding: 4px 5px;
  cursor: pointer;
}

.modal__content {
  min-height: 449px;
}

.modal__footer {
  position: relative;
  padding-top: var(--modal-window-padding);
  display: flex;
  justify-content: flex-end;

  &:before {
    content: '';
    position: absolute;
    top: 0;
    left: var(--modal-window-padding-negative);
    right: var(--modal-window-padding-negative);
    background: #eaecf0;
    height: 1px;
    border-radius: 1px;
  }
}
</style>