<template>
  <div class="home">
    <button
      class="home__button button"
      @click="isModalVisible = !isModalVisible"
    >
      Новый озыв
    </button>

    <teleport to="#modal-container">
      <Modal
        title="Новый отзыв"
        buttonText="Продолжить"
        :isVisible="isModalVisible"
        :isPageType="true"
        @close="isModalVisible = false"
      >
        Text
      </Modal>
    </teleport>
  </div>
</template>

<script lang="ts">
import Modal from '@/components/Modal.vue';
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'Home',
  components: { Modal },
  data: () => ({
    isModalVisible: true
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
