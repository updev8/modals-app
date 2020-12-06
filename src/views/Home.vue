<template>
  <div class="home">
    <button class="button" @click="isModalVisible = !isModalVisible">
      Новый отзыв
    </button>

    <Modal
      title="Новый отзыв"
      buttonText="Продолжить"
      :isVisible="isModalVisible"
      :isPageType="isModalPageType"
      @close="isModalVisible = false"
      @next="isModalPageType = !isModalPageType"
    >
      <Review :step="2" />
    </Modal>
  </div>
</template>

<script lang="ts">
import Modal from '@/components/Modal.vue';
import Review from '@/components/Review.vue';
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'Home',
  components: { Modal, Review },
  data: () => ({
    isModalVisible: true,
    isModalPageType: false
  })
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
