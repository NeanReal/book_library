<script setup>
import { RouterLink } from 'vue-router'

defineProps({
  book: {
    type: Object,
    default: null,
  },
})
</script>

<template>
  <article v-if="book" class="detail-card">
    <div class="detail-card__header">
      <div class="stack stack--small">
        <p class="eyebrow">Карточка книги</p>
        <h1>{{ book.title }}</h1>
        <p class="lead lead--compact">{{ book.author }}</p>
      </div>

      <RouterLink class="button button--ghost button--small" to="/books">
        Назад к каталогу
      </RouterLink>
    </div>

    <div class="detail-grid">
      <div class="detail-copy">
        <p class="detail-copy__text">{{ book.description }}</p>
      </div>

      <div class="detail-meta">
        <div class="meta-row">
          <span>Категория</span>
          <strong>{{ book.category }}</strong>
        </div>
        <div class="meta-row">
          <span>Год издания</span>
          <strong>{{ book.year }}</strong>
        </div>
        <div class="meta-row">
          <span>Количество страниц</span>
          <strong>{{ book.pages }}</strong>
        </div>
        <div class="meta-row">
          <span>Маршрут</span>
          <strong>/books/{{ book.id }}</strong>
        </div>
      </div>
    </div>
  </article>

  <article v-else class="empty-state empty-state--large">
    <h1>Книга не найдена</h1>
    <p>Проверьте адрес страницы или вернитесь в каталог, чтобы выбрать существующую книгу.</p>
    <RouterLink class="button" to="/books">Вернуться к каталогу</RouterLink>
  </article>
</template>
