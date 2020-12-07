<template>
  <div class="home">
    <button class="button" @click="isModalVisible = !isModalVisible">
      Новый отзыв
    </button>

    <Modal
      title="Новый отзыв"
      :isVisible="isModalVisible"
      :isTypePage="isModalTypePage"
      :buttonText="modalButtonText"
      @close="isModalVisible = false"
      @back="onModalBackClick"
      @next="onModalNextClick"
    >
      <Review :isTablet="isTablet" :step="step" :isSubmit="isSubmit" />
    </Modal>

    <Toast msg="Спасибо, отзыв опубликован!" variant="success" />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { debounce } from 'lodash';
import Modal from '@/components/Modal.vue';
import Review from '@/components/Review.vue';
import Toast from '@/components/Toast.vue';

export default defineComponent({
  name: 'Home',
  components: { Modal, Review, Toast },
  data: () => ({
    step: 2,
    amountOfSteps: 2,
    isTablet: false,
    isModalVisible: true,
    isSubmit: false
  }),

  computed: {
    modalButtonText(): string {
      if (!this.isTablet && this.step < this.amountOfSteps) {
        return 'Продолжить';
      } else {
        return 'Отправить';
      }
    },
    isModalTypePage(): boolean {
      if (!this.isTablet && this.step > 1) return true;
      return false;
    }
  },

  methods: {
    onWindowResize() {
      if (window.innerWidth >= 650) {
        this.isTablet = true;
      } else {
        this.isTablet = false;
      }
    },
    onModalNextClick() {
      if (!this.isTablet && this.step < this.amountOfSteps) {
        this.step += 1;
      } else {
        this.isSubmit = true;
        setTimeout(() => (this.isSubmit = false), 50);
      }
    },
    onModalBackClick() {
      if (this.step !== 1) {
        this.step -= 1;
      }
    }
  },

  mounted() {
    this.onWindowResize();
    window.addEventListener('resize', debounce(this.onWindowResize, 100));
  },

  beforeUnmount() {
    window.removeEventListener('resize', this.onWindowResize);
  }
});
</script>

<style lang="scss">
.home {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 25px 0;
}
</style>
