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
      Text
    </Modal>
  </div>
</template>

<script lang="ts">
import Modal from '@/components/Modal.vue';
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'Home',
  components: { Modal },
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
