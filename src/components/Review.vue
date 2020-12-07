<template>
  <div class="review">
    <div
      class="review__step"
      :class="{ 'review__step--visible': isTablet || step === 1 }"
    >
      <div class="review__header">
        <img
          class="review__image"
          width="84"
          height="56"
          :alt="title"
          :src="imgSrc"
        />
        <div class="review__header-wrapper">
          <h3 class="review__title">{{ title }}</h3>
          <p class="review__author">{{ author }}</p>
        </div>
      </div>
      <div class="review__ratings">
        <Rating
          class="review__rating"
          v-for="{ id, title, lenght, value } in ratings"
          :key="id"
          :id="id"
          :title="title"
          :length="lenght"
          :value="value"
          @selectedValue="selectRatingValue"
        />
      </div>
    </div>
    <div
      class="review__step"
      :class="{ 'review__step--visible': isTablet || step === 2 }"
    >
      <div class="review__field">
        <textarea
          class="review__textarea"
          placeholder="Комментарий"
          v-model="textAreaValue"
          :maxlength="textAreaValueMaxLenght"
        />
        <span class="review__textarea-count">
          {{ textAreaValueLenght }}/{{ textAreaValueMaxLenght }}
        </span>
      </div>
      <Images
        class="review__images"
        @onAddImages="onAddImages"
        @onDeleteImage="onDeleteImage"
        :isClean="wasSubmittedSuccessfuly"
      />
    </div>
    <Toast
      :isVisible="toast.isVisible"
      :msg="toast.msg"
      :variant="toast.variant"
      @close="toast.isVisible = false"
    />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import axios from 'axios';
import Rating from '@/components/Rating.vue';
import Images from '@/components/Images.vue';
import Toast from './Toast.vue';

export default defineComponent({
  name: 'Review',
  components: { Rating, Images, Toast },
  emits: ['submitted', 'goToStep'],
  props: {
    title: { type: String, required: true },
    author: { type: String, required: true },
    imgSrc: { type: String, required: true },
    step: { type: Number, default: 1 },
    isTablet: Boolean,
    isSubmit: { type: Boolean, default: false }
  },
  data() {
    return {
      ratings: [
        { id: 1, title: 'Скорость', lenght: 5, value: 0 },
        { id: 2, title: 'Скорость отдачи видео', lenght: 5, value: 0 },
        { id: 3, title: 'Качество', lenght: 5, value: 0 },
        { id: 4, title: 'Исполнитель солнышка?', lenght: 5, value: 0 }
      ],
      images: [] as File[],
      textAreaValue: '',
      textAreaValueMaxLenght: 500,

      toast: {
        isVisible: false,
        msg: '',
        variant: 'error'
      },
      wasSubmittedSuccessfuly: false
    };
  },
  watch: {
    isSubmit(newIsSubmit) {
      if (newIsSubmit) this.handleSubmit();
    },

    wasSubmittedSuccessfuly() {
      setTimeout(() => (this.wasSubmittedSuccessfuly = false), 500);
    },

    'toast.isVisible': function (newIsVisible) {
      if (newIsVisible) {
        setTimeout(() => (this.toast.isVisible = false), 4000);
      }
    }
  },

  computed: {
    textAreaValueLenght(): number {
      return this.textAreaValue.length;
    }
  },

  methods: {
    onAddImages(images: File[]) {
      this.images = [...this.images, ...images];
    },

    onDeleteImage(index: number) {
      this.images = this.images.filter((image, idx) => index !== idx);
    },

    selectRatingValue({ value, id }: { value: number; id: number }) {
      this.ratings = this.ratings.map((rating) => {
        if (rating.id === id) return { ...rating, value };
        return rating;
      });
    },

    createFormData() {
      const formData = new FormData();

      formData.append('title', this.title);
      formData.append('author', this.author);
      formData.append('textAreaValue', this.textAreaValue);
      formData.append('ratings', JSON.stringify(this.ratings));

      // the api doesn't support binary formats
      // this.images.forEach((image) =>
      //   formData.append('images', image, image.name)
      // );

      return { formData };
    },

    handleSubmitted(wasSuccessful = false) {
      this.$emit('submitted', wasSuccessful);

      if (wasSuccessful) {
        this.wasSubmittedSuccessfuly = true;
        this.textAreaValue = '';
        this.images = [];
        this.ratings = this.ratings.map((rating) => ({ ...rating, value: 0 }));
      }
    },

    async uploadToApi() {
      try {
        const { formData } = this.createFormData();
        const url = 'https://jsonplaceholder.typicode.com/posts';
        await axios.post(url, formData);

        this.handleSubmitted(true);
      } catch (e) {
        this.handleSubmitted(false);
      }
    },

    handleSubmit() {
      const areRatingsFilled = this.ratings.every((rating) => rating.value > 0);
      const areAllFilled =
        this.images.length && this.textAreaValue && areRatingsFilled;
      this.toast.isVisible = true;

      if (areAllFilled) {
        this.uploadToApi();
        this.toast.isVisible = false;
      } else if (!areRatingsFilled) {
        this.toast.msg = 'Должны быть проставленые все оценки';
        this.$emit('goToStep', 1);
      } else if (!this.textAreaValue) {
        this.toast.msg = 'Комментарий не должен быть пустым';
      } else if (!this.images.length) {
        this.toast.msg = 'Минимум 1 изображение должно быть добавлено';
      }
    }
  }
});
</script>

<style lang="scss">
.review__step {
  display: none;

  &--visible {
    display: block;
  }
}

.review__header {
  @include tablet {
    display: flex;
    flex-wrap: wrap;
  }
}

.review__image {
  border-radius: 6px;

  @include tablet {
    width: 102px;
    height: 68px;
  }
}

.review__header-wrapper {
  @include tablet {
    margin-left: 20px;
  }
}

.review__title {
  font-weight: var(--fw-semi-bold);
  font-size: 16px;
  line-height: 24px;
  margin: 0;
  margin-top: 12px;
}

.review__author {
  font-size: 12px;
  line-height: 16px;
  margin: 2px 0;
}

.review__ratings {
  padding-top: 23px;

  @include tablet {
    padding-top: 31px;
    display: flex;
    flex-wrap: wrap;
  }
}

.review__rating {
  margin-bottom: 11px;

  @include tablet {
    width: 44.3%;
    margin-bottom: 15px;
  }
}

.review__field {
  @include tablet {
    padding-top: 7px;
  }
}

.review__textarea {
  resize: none;
  border: 1px solid var(--c-input-border-primary);
  background: var(--c-input-bg-primary);
  padding: 20px 11px;
  border-radius: 6px;

  width: 100%;
  height: 146px;

  &::placeholder,
  &:-ms-input-placeholder,
  &::-ms-input-placeholder {
    color: var(--c-input-text-primary);
    opacity: 1;
  }

  @include tablet {
    padding: 16px 11px;
    height: 100px;
  }
}

.review__textarea-count {
  display: block;
  text-align: end;
  color: var(--c-input-text-primary);
  font-size: 12px;
  line-height: 16px;
}

.review__images {
  margin-top: 18px;
}
</style>
