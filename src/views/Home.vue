<template>
  <div class="home">
    <button class="button" @click="isModalVisible = !isModalVisible">
      Новый отзыв
    </button>

    <Modal
      title="Новый отзыв"
      :isVisible="isModalVisible"
      :isPageType="isModalPageType"
      :buttonText="modalButtonText"
      @close="isModalVisible = false"
      @back="step !== 1 && (step -= 1)"
      @next="step < amountOfSteps && (step += 1)"
    >
      <Review :isTablet="isTablet" :step="step" />
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
    isModalVisible: true
  }),

  computed: {
    modalButtonText(): string {
      if (!this.isTablet && this.step < this.amountOfSteps) {
        return 'Продолжить';
      } else {
        return 'Отправить';
      }
    },
    isModalPageType(): boolean {
      if (!this.isTablet && this.step > 1) return true;
      return false;
    }
  },

  methods: {
    onResize() {
      if (window.innerWidth >= 650) {
        this.isTablet = true;
      } else {
        this.isTablet = false;
      }
    }
  },

  mounted() {
    this.onResize();
    window.addEventListener('resize', debounce(this.onResize, 100));
  },

  beforeUnmount() {
    window.removeEventListener('resize', this.onResize);
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
