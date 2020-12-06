<template>
  <div class="review">
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
      <Rating class="review__rating" title="Скорость" :length="5" :value="3" />
      <Rating
        class="review__rating"
        title="Скорость отдачи видео"
        :length="5"
        :value="3"
      />
      <Rating
        class="review__rating"
        title="Исполнитель солнышка?"
        :length="5"
        :value="3"
      />
      <Rating
        class="review__rating"
        title="Исполнитель солнышка?"
        :length="5"
        :value="3"
      />
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import Rating from '@/components/Rating.vue';

export default defineComponent({
  name: 'Review',
  components: { Rating }
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
</style>
