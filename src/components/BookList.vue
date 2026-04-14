<script setup>
import BookItem from './BookItem.vue'

defineProps({
  books: {
    type: Array,
    default: () => [],
  },
})
</script>

<template>
  <div v-if="books.length" class="book-grid">
    <BookItem v-for="book in books" :key="book.id" :book="book" />
  </div>
  <div v-else class="empty-state">
    <h3>Книги не найдены</h3>
    <p>Выберите другую категорию или добавьте новую книгу через форму справа.</p>
  </div>
</template>
