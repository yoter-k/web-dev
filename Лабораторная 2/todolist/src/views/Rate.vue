<script setup>
import { ref } from "vue";

const rating = ref(0);

const setRating = (value) => {
  rating.value = value;
};

const getRatingMessage = () => {
  switch (rating.value) {
    case 1:
      return "Очень плохо";
    case 2:
      return "Плохо";
    case 3:
      return "Удовлетворительно";
    case 4:
      return "Хорошо";
    case 5:
      return "Отлично";
    default:
      return "";
  }
};
</script>

<template>
  <div class="container py-4">
    <h1>Оценить сайт</h1>

    <div class="star-rating mb-4">
      <span
        v-for="star in 5"
        :key="star"
        class="star"
        :class="{ active: star <= rating }"
        @click="setRating(star)"
      >
        ★
      </span>
    </div>

    <div v-if="rating > 0" class="alert alert-info">
      {{ getRatingMessage() }}
    </div>
  </div>
</template>

<style scoped>
.star-rating {
  font-size: 5rem;
  cursor: pointer;
}

.star {
  color: #ccc;
  transition: color 0.3s;
  margin: 0 5px;
}

.star.active {
  color: #ffd700;
}

.alert {
  margin-top: 1rem;
}
</style>
