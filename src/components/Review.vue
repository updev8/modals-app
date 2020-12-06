<template>
  <div class="review">
    <div class="review__wrapper" v-if="step === 1">
      <div class="review__header">
        <img
          class="review__image"
          width="86"
          height="56"
          alt=""
          src="@/assets/wedding.jpg"
        />
        <h3 class="review__title">Фоточки в свадебном платьице</h3>
        <p class="review__author">Алена Смирнова</p>
      </div>
      <div class="review__ratings">
        <Rating
          class="review__rating"
          v-for="rating in ratings"
          :key="rating.id"
          :title="rating.title"
          :length="rating.lenght"
          :value="rating.value"
        />
      </div>
    </div>
    <div class="review__wrapper" v-if="step === 2">
      <div class="review__field">
        <textarea class="review__textarea" placeholder="Комментарий"></textarea>
        <span class="review__textarea-count">12/500</span>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import Rating from '@/components/Rating.vue';

export default defineComponent({
  name: 'Review',
  components: { Rating },
  props: {
    step: Number
  },
  data() {
    return {
      ratings: [
        { id: 1, title: 'Скорость', lenght: 5, value: 0 },
        { id: 2, title: 'Скорость отдачи видео', lenght: 5, value: 3 },
        { id: 3, title: 'Исполнитель солнышка?', lenght: 5, value: 5 },
        { id: 4, title: 'Исполнитель солнышка?', lenght: 5, value: 3 }
      ]
    };
  }
});
</script>

<style lang="scss">
.review__title {
  font-weight: var(--fw-semi-bold);
  font-size: 16px;
  line-height: 24px;
  margin: 0;
  margin-top: 15px;
}

.review__author {
  font-size: 12px;
  line-height: 16px;
  margin: 2px 0;
}

.review__ratings {
  padding-top: 16px;
}

.review__rating {
  margin-bottom: 8px;
}

.review__textarea {
  resize: none;
  border: 1px solid var(--c-input-border-primary);
  background: var(--c-input-bg-primary);
  padding: 16px 12px;

  width: 100%;
  height: 146px;
}

.review__textarea-count {
  display: block;
  margin-top: 4px;
  text-align: end;
  color: var(--c-input-text-primary);
  font-size: 12px;
  line-height: 16px;
}
</style>