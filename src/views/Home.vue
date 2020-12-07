<template>
  <div class="home">
    <button class="button" @click="isModalVisible = !isModalVisible">
      Новый отзыв
    </button>

    <Modal
      title="Мой отзыв"
      :isVisible="isModalVisible"
      :isTypePage="isModalTypePage"
      :buttonText="modalButtonText"
      @close="isModalVisible = false"
      @back="onModalBackClick"
      @next="onModalNextClick"
    >
      <Review
        title="Фоточки в свадебном платьице"
        author="Алена Смирнова"
        imgSrc="/images/wedding.jpg"
        :isTablet="isTablet"
        :step="step"
        :isSubmit="isSubmit"
        @submitted="onReviewSubmitted"
      />
    </Modal>

    <Toast
      :isVisible="isToastVisible"
      :msg="wasSubmittedSuccessfuly ? 'Спасибо, отзыв опубликован' : 'Ошибка'"
      :variant="wasSubmittedSuccessfuly ? 'success' : 'error'"
      @close="isToastVisible = false"
    />
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
    step: 1,
    amountOfSteps: 2,
    isTablet: false,
    isModalVisible: true,
    isSubmit: false,

    isToastVisible: false,
    wasSubmittedSuccessfuly: false
  }),
  watch: {
    isToastVisible() {
      setTimeout(() => {
        // toggle off the toast
        this.isToastVisible = false;
      }, 5000);
    }
  },
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
      if (window.innerWidth >= 560) {
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
        setTimeout(() => (this.isSubmit = false), 500);
      }
    },
    onModalBackClick() {
      if (this.step !== 1) {
        this.step -= 1;
      }
    },
    onReviewSubmitted(wasSuccessful = false) {
      if (wasSuccessful) {
        this.isModalVisible = false;
      }

      this.isToastVisible = true;
      this.wasSubmittedSuccessfuly = wasSuccessful;
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
