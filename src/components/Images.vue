<template>
  <div class="images">
    <input
      class="images__input"
      type="file"
      ref="imagesUploadInput"
      multiple
      accept=".png,.jpg,.jpeg"
      @change="onFileSelected"
    />
    <button class="images__add" @click="$refs.imagesUploadInput.click()">
      <IconPlus />
    </button>
    <div
      class="images__wrapper"
      v-for="imageBlob in imageBlobs"
      :key="imageBlob"
    >
      <img class="images__image" :src="imageBlob" />
      <button class="images__delete" @click="onDelete(imageBlob)">
        <IconBin />
      </button>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import IconBin from './icons/IconBin.vue';
import IconPlus from './icons/IconPlus.vue';

export default defineComponent({
  name: 'Images',
  components: { IconPlus, IconBin },
  data() {
    return {
      imageBlobs: [] as string[]
    };
  },
  methods: {
    onFileSelected(e: Event) {
      const target = e.target as HTMLInputElement;
      if (target.files?.length) {
        const files = Array.from(target.files);
        const imageBlobs = files.map((file) => URL.createObjectURL(file));
        this.imageBlobs = [...this.imageBlobs, ...imageBlobs];
      }
    },
    onDelete(value: string) {
      this.imageBlobs = this.imageBlobs.filter((blob) => blob !== value);
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

.images__add {
  cursor: pointer;
}

.images__wrapper {
  position: relative;
  width: 80px;
  height: 80px;
}

.images__image {
  border: none;
  object-fit: cover;
}

.images__delete {
  position: absolute;
  @include reset-button;
  background: rgba(0, 0, 0, 0.6);
  width: 32px;
  height: 32px;
  top: 8px;
  right: 8px;
  border-radius: 4px;
  opacity: 0;
  cursor: pointer;

  &:focus,
  &:hover {
    opacity: 1;
  }
}

.images__image:hover ~ .images__delete {
  opacity: 1;
}
</style>
