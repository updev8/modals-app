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
        <h3 class="review__title">{{ title }}</h3>
        <p class="review__author">{{ author }}</p>
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
      <Images class="review__images" @onSubmit="onAddImages" />
    </div>
    <Toast
      :isVisible="wasSubmitted"
      :msg="wasSubmittedSuccessfuly ? 'Спасибо, отзыв опубликован' : 'Ошибка'"
      :variant="wasSubmittedSuccessfuly ? 'success' : 'error'"
      @close="wasSubmitted = false"
    />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import axios from 'axios';
import Rating from '@/components/Rating.vue';
import Images from '@/components/Images.vue';
import Toast from '@/components/Toast.vue';

export default defineComponent({
  name: 'Review',
  components: { Rating, Images, Toast },
  props: {
    title: String,
    author: String,
    imgSrc: { type: String, required: true },
    step: { type: Number, default: 1 },
    isTablet: Boolean,
    isSubmit: { type: Boolean, default: false }
  },
  data() {
    return {
      ratings: [
        { id: 1, title: 'Скорость', lenght: 5, value: 0 },
        { id: 2, title: 'Скорость отдачи видео', lenght: 5, value: 3 },
        { id: 3, title: 'Исполнитель солнышка?', lenght: 5, value: 5 },
        { id: 4, title: 'Исполнитель солнышка?', lenght: 5, value: 3 }
      ],
      images: [] as File[],
      textAreaValue: '',
      textAreaValueMaxLenght: 500,
      wasSubmitted: false,
      wasSubmittedSuccessfuly: false
    };
  },
  watch: {
    isSubmit(newIsSubmit) {
      if (newIsSubmit) this.handleSubmit();
    }
  },
  computed: {
    textAreaValueLenght(): number {
      return this.textAreaValue.length;
    }
  },
  methods: {
    async handleSubmit() {
      try {
        const { formData } = this.createFormData();
        const url = 'https://jsonplaceholder.typicode.com/posts';
        await axios.post(url, formData, {
          headers: { 'Content-type': 'application/json; charset=UTF-8' }
        });

        this.setWasSubmittedSuccessfuly(true);
      } catch (e) {
        this.setWasSubmittedSuccessfuly(false);
      }
    },

    setWasSubmittedSuccessfuly(wasSuccessful = false) {
      this.wasSubmitted = true;
      this.wasSubmittedSuccessfuly = wasSuccessful;
    },

    createFormData() {
      const formData = new FormData();

      formData.append('title', this.title as string);
      formData.append('author', this.author as string);
      formData.append('textAreaValue', this.textAreaValue);
      formData.append('ratings', JSON.stringify(this.ratings));

      // the api doesn't support binary formats
      // this.images.forEach((image) =>
      //   formData.append('images', image, image.name)
      // );

      return { formData };
    },

    onAddImages(images: File[]) {
      this.images = [...this.images, ...images];
    },

    selectRatingValue({ value, id }: { value: number; id: number }) {
      this.ratings = this.ratings.map((rating) => {
        if (rating.id === id) return { ...rating, value };
        return rating;
      });
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

.review__image {
  border-radius: 6px;
}

.review__title {
  font-weight: var(--fw-semi-bold);
  font-size: 16px;
  line-height: 24px;
  margin: 0;
  margin-top: 11px;
}

.review__author {
  font-size: 12px;
  line-height: 16px;
  margin: 2px 0;
}

.review__ratings {
  padding-top: 23px;

  @include tablet {
    display: flex;
    flex-wrap: wrap;
  }
}

.review__rating {
  margin-bottom: 11px;

  @include tablet {
    width: 50%;
  }
}

.review__field {
  padding-top: 14px;
}

.review__textarea {
  resize: none;
  border: 1px solid var(--c-input-border-primary);
  background: var(--c-input-bg-primary);
  padding: 20px 12px;

  width: 100%;
  height: 146px;

  &::placeholder,
  &:-ms-input-placeholder,
  &::-ms-input-placeholder {
    color: var(--c-input-text-primary);
    opacity: 1;
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
