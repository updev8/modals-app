<template>
  <div class="rating">
    <h4 class="rating__title">{{ title }}</h4>
    <div class="rating__stars">
      <button
        class="rating__star"
        :class="{
          'rating__star--active': star <= (isHovering ? valueHovering : value)
        }"
        v-for="star in stars"
        :key="star"
        @click="onClick()"
        @mouseenter="onMouseEnter(star)"
        @mouseleave="onMouseLeave"
      >
        <IconStar />
      </button>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import IconStar from '@/components/icons/IconStar.vue';

export default defineComponent({
  name: 'Rating',
  components: { IconStar },
  props: {
    title: String,
    length: { type: Number, default: 3, required: true },
    value: { type: Number, default: 0, required: true },
    id: Number
  },
  emits: ['selectedValue'],

  data() {
    return {
      valueHovering: 0,
      isHovering: false
    };
  },

  computed: {
    stars(): number[] {
      return Array.from({ length: this.length }, (value, index) => index + 1);
    }
  },

  methods: {
    onMouseEnter(value: number) {
      this.isHovering = true;
      this.valueHovering = value;
    },
    onClick() {
      this.$emit('selectedValue', { value: this.valueHovering, id: this.id });
    },
    onMouseLeave() {
      this.isHovering = false;
    }
  }
});
</script>

<style lang="scss">
.rating__title {
  margin: 0;
  margin-bottom: 8px;

  font-weight: var(--fw-medium);
  font-size: 14px;
  line-height: 20px;
}

.rating__star {
  @include reset-button;
  width: 32px;
  height: 32px;
  margin-right: 4px;
  margin-bottom: 5px;

  cursor: pointer;

  &:hover path,
  &--active path {
    fill: var(--c-accent-bg-primary);
  }
}
</style>
