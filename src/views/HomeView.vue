<script setup>
import { computed, inject } from 'vue'
import { RouterLink } from 'vue-router'

const library = inject('library')

if (!library) {
  throw new Error('Library provider is missing')
}

const bookCount = computed(() => library.books.value.length)
const categoryCount = computed(() => library.categories.value.length)
const newestBook = computed(() => library.books.value[0] ?? null)
const highlightedCategory = computed(() => {
  const totals = library.books.value.reduce((accumulator, book) => {
    accumulator[book.category] = (accumulator[book.category] || 0) + 1
    return accumulator
  }, {})

  const [categoryName] =
    Object.entries(totals).sort((left, right) => right[1] - left[1])[0] ?? []

  return categoryName ?? 'Без категории'
})
</script>

<template>
  <section class="page">
    <div class="hero-panel">
      <div class="hero-copy">
        <p class="eyebrow">Vue.js • Vue Router • MVP</p>
        <h1>Онлайн-библиотека с динамическими маршрутами и компонентной структурой</h1>
        <p class="lead">
          Проект показывает главную страницу, каталог с фильтрами, карточки книг, отдельную
          страницу книги и форму добавления новой записи.
        </p>

        <div class="button-row">
          <RouterLink class="button" to="/books">Открыть каталог</RouterLink>
          <RouterLink
            class="button button--ghost"
            :to="{ name: 'books-by-category', params: { category: highlightedCategory } }"
          >
            Перейти в раздел «{{ highlightedCategory }}»
          </RouterLink>
        </div>
      </div>

      <div class="hero-note">
        <p class="note-label">Автор проекта</p>
        <p class="note-value">{{ library.libraryOwner.name }}</p>
        <p class="note-caption">ИГУ, группа {{ library.libraryOwner.group }}</p>
      </div>
    </div>

    <div class="metrics">
      <article class="metric-card">
        <span class="metric-card__value">{{ bookCount }}</span>
        <p class="metric-card__label">книг уже добавлено</p>
      </article>
      <article class="metric-card">
        <span class="metric-card__value">{{ categoryCount }}</span>
        <p class="metric-card__label">категорий доступно</p>
      </article>
      <article class="metric-card">
        <span class="metric-card__value">{{ highlightedCategory }}</span>
        <p class="metric-card__label">раздел с наибольшим числом книг</p>
      </article>
    </div>

    <div class="home-grid">
      <article class="section-card">
        <p class="eyebrow">Что реализовано</p>
        <h2>Функции MVP</h2>
        <ul class="feature-list">
          <li>главная страница с описанием проекта;</li>
          <li>список книг и фильтрация по категории;</li>
          <li>динамический маршрут для страницы книги;</li>
          <li>динамический маршрут для страницы раздела;</li>
          <li>форма добавления новой книги.</li>
        </ul>
      </article>

      <article class="section-card section-card--accent">
        <p class="eyebrow">Последнее пополнение</p>
        <h2>{{ newestBook?.title || 'Каталог пока пуст' }}</h2>
        <p class="lead lead--compact">
          {{ newestBook?.description || 'Добавьте первую книгу через форму на странице каталога.' }}
        </p>
        <RouterLink
          v-if="newestBook"
          class="button button--small"
          :to="{ name: 'book-detail', params: { id: newestBook.id } }"
        >
          Читать подробнее
        </RouterLink>
      </article>
    </div>
  </section>
</template>
