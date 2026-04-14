<script setup>
import { computed } from 'vue'
import { RouterLink } from 'vue-router'

const props = defineProps({
  book: {
    type: Object,
    required: true,
  },
})

const preview = computed(() => {
  if (props.book.description.length <= 125) {
    return props.book.description
  }

  return `${props.book.description.slice(0, 122)}...`
})
</script>

<template>
  <article class="book-card">
    <div class="book-card__top">
      <span class="badge">{{ book.category }}</span>
      <span class="book-card__meta">{{ book.year }}</span>
    </div>

    <div class="stack stack--small">
      <h3>{{ book.title }}</h3>
      <p class="book-card__author">{{ book.author }}</p>
      <p>{{ preview }}</p>
    </div>

    <div class="book-card__footer">
      <span class="book-card__meta">{{ book.pages }} стр.</span>
      <RouterLink class="button button--small" :to="{ name: 'book-detail', params: { id: book.id } }">
        Подробнее
      </RouterLink>
    </div>
  </article>
</template>
