<template>
  <div class="images">
    <input
      class="images__input"
      type="file"
      ref="imagesUploadInput"
      @change="onFileSelected"
    />
    <button class="images__add" @click="$refs.imagesUploadInput.click()">
      <IconPlus />
    </button>
    <img
      class="images__image"
      v-for="selectedImage in selectedImages"
      :key="selectedImage"
      :src="selectedImage"
    />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import IconPlus from './icons/IconPlus.vue';

export default defineComponent({
  name: 'Images',
  components: { IconPlus },
  data() {
    return {
      selectedImages: [[]]
    } as { selectedImages: [string[]] };
  },
  methods: {
    onFileSelected(e: Event) {
      const target = e.target as HTMLInputElement;
      if (target.files?.length) {
        const files = Array.from(target.files);
        const filesUrls = files.map((file) => URL.createObjectURL(file));
        this.selectedImages.push(filesUrls);
        // this.selectedImages = URL.createObjectURL(file);
      }
    }
  }
});
</script>

<style lang="scss">
.images {
  margin-top: 16px;
  display: flex;
  flex-wrap: wrap;
}

.images__input {
  display: none;
}

.images__add,
.images__image {
  @include reset-button;
  width: 80px;
  height: 80px;
  border: 1px solid var(--c-accent-bg-secondary);
  background: var(--c-accent-bg-quaternary);
  margin-right: 8px;
  margin-bottom: 8px;
  border-radius: 4px;
}

.images__image {
  border: none;
  object-fit: cover;
}
</style>
