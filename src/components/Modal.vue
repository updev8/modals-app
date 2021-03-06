<template>
  <teleport to="#modal-container">
    <div
      class="modal"
      :class="{ 'modal--type-page': isTypePage }"
      v-if="isVisible"
    >
      <div class="modal__window">
        <div class="modal__header">
          <button class="modal__back" @click="$emit('back')">
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
          <button class="modal__button button" @click="$emit('next')">
            {{ buttonText }}
          </button>
        </div>
      </div>
    </div>
  </teleport>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import IconBack from './icons/IconBack.vue';
import IconClose from './icons/IconClose.vue';

export default defineComponent({
  name: 'Modal',
  components: { IconClose, IconBack },
  emits: ['back', 'close', 'next'],
  props: {
    isVisible: Boolean,
    isTypePage: Boolean,
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
  background: var(--c-bg-overlay-primary);

  display: flex;
  justify-content: center;

  overflow-y: auto;
  padding-top: 8.9vh;

  @include tablet {
    padding: 0;
    align-items: center;
  }

  &--type-page {
    padding: 0;
  }
}

.modal__window {
  --modal-window-padding: 16px;
  --modal-window-padding-negative: calc(var(--modal-window-padding) * -1);

  margin-top: auto;
  background: var(--c-bg-modal-primary);
  border-radius: 16px 16px 0px 0px;
  padding: var(--modal-window-padding);
  min-width: 320px;
  max-width: 560px;
  position: relative;

  display: flex;
  flex-direction: column;

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

  @include tablet {
    --modal-window-padding: 32px;

    padding-bottom: 16px;
    margin-top: unset;
    border-radius: 16px;

    &::before {
      display: none;
    }
  }
}

.modal--type-page .modal__window {
  border-radius: unset;
  width: 100%;
  min-height: 100%;

  &::before {
    display: none;
  }
}

.modal__header {
  display: flex;
  align-items: center;

  @include tablet {
    padding-bottom: 13px;
  }
}

.modal--type-page .modal__header {
  padding-bottom: 13px;
}

.modal__back {
  display: none;
}

.modal--type-page .modal__back {
  display: inline-block;
  @include reset-button;
  padding-top: 4px;
  padding-right: 16px;
  padding-left: 4px;
  cursor: pointer;
}

.modal__title {
  margin: 0;
  font-weight: var(--fw-semi-bold);
  font-size: 16px;
  line-height: 24px;

  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.modal__close {
  @include reset-button;
  margin-left: auto;
  padding: 4px 5px;
  cursor: pointer;

  @include tablet {
    position: relative;
    right: -16px;
  }
}

@mixin modal-line {
  position: relative;

  &::before {
    content: '';
    position: absolute;
    top: 0;
    left: var(--modal-window-padding-negative);
    right: var(--modal-window-padding-negative);
    background: var(--c-accent-bg-secondary);
    height: 1px;
    border-radius: 1px;
  }
}

.modal__content {
  padding-top: 13px;
  padding-bottom: 8px;

  @include tablet {
    @include modal-line;
    padding-top: 26px;
    padding-bottom: 15px;
  }
}

.modal--type-page .modal__content {
  @include modal-line;
}

.modal__footer {
  position: relative;
  padding-top: var(--modal-window-padding);
  display: flex;
  justify-content: flex-end;
  @include modal-line;

  @include tablet {
    padding-top: 16px;
  }
}

.modal--type-page .modal__footer {
  margin-top: auto;
}

.modal__button {
  @include tablet {
    position: relative;
    right: -16px;
  }
}
</style>
