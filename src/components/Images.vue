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
      v-for="(imageBlob, index) in imageBlobs"
      :key="imageBlob"
    >
      <img class="images__image" :src="imageBlob" />
      <button class="images__delete" @click="onDelete(imageBlob, index)">
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
  props: {
    isClean: { type: Boolean, default: false }
  },
  emits: ['onAddImages', 'onDeleteImage'],
  data() {
    return {
      imageFiles: null as FileList | null,
      imageBlobs: [] as string[]
    };
  },
  watch: {
    isClean(newIsClean) {
      if (newIsClean) {
        this.imageBlobs = [];
      }
    }
  },
  methods: {
    onFileSelected(e: Event) {
      const { files } = e.target as HTMLInputElement;

      if (files?.length) {
        this.createBlobs(files);
        this.$emit('onAddImages', files);
      }
    },

    createBlobs(files: FileList) {
      const images = Array.from(files);
      const imageBlobs = images.map((image) => URL.createObjectURL(image));
      this.imageBlobs = [...this.imageBlobs, ...imageBlobs];
    },

    onDelete(imageBlob: string, index: number) {
      this.imageBlobs = this.imageBlobs.filter((blob) => blob !== imageBlob);
      this.$emit('onDeleteImage', index);
    }
  }
});
</script>

<style lang="scss">
.images {
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

  &:hover {
    transform: scale(1.02);
  }
}

.images__add {
  cursor: pointer;
}

.images__wrapper {
  position: relative;
  width: 80px;
  height: 80px;
  margin-right: 8px;
}

.images__image {
  border: none;
  object-fit: cover;
}

.images__delete {
  @include reset-button;
  position: absolute;
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
