<script setup>
import { computed, provide, ref } from 'vue'
import { RouterLink, RouterView } from 'vue-router'

const libraryOwner = {
  name: 'Голыгин Илья Викторович',
  group: '2362-ДБ',
}

const books = ref([
  {
    id: 1,
    title: 'Мастер и Маргарита',
    author: 'Михаил Булгаков',
    category: 'Классика',
    year: 1967,
    pages: 480,
    description:
      'Роман о добре и зле, свободе выбора и цене правды. В центре истории переплетаются Москва, Ершалаим и мистический бал у Воланда.',
  },
  {
    id: 2,
    title: '451 градус по Фаренгейту',
    author: 'Рэй Брэдбери',
    category: 'Фантастика',
    year: 1953,
    pages: 256,
    description:
      'Антиутопия о мире, где книги запрещены, а пожарные не тушат огонь, а сжигают библиотеки. История о сопротивлении и памяти.',
  },
  {
    id: 3,
    title: 'Краткая история времени',
    author: 'Стивен Хокинг',
    category: 'Наука',
    year: 1988,
    pages: 256,
    description:
      'Популярное введение в космологию: черные дыры, происхождение Вселенной, природа времени и попытка объяснить сложное простым языком.',
  },
  {
    id: 4,
    title: 'Атлант расправил плечи',
    author: 'Айн Рэнд',
    category: 'Бизнес',
    year: 1957,
    pages: 1136,
    description:
      'Объемный роман о предпринимательстве, свободе выбора и личной ответственности. Подходит для раздела деловой литературы.',
  },
  {
    id: 5,
    title: 'Сапиенс. Краткая история человечества',
    author: 'Юваль Ной Харари',
    category: 'История',
    year: 2011,
    pages: 512,
    description:
      'Обзор ключевых этапов развития человека: от охотников-собирателей до цифровой эпохи. Книга связывает историю, культуру и экономику.',
  },
  {
    id: 6,
    title: 'Гарри Поттер и философский камень',
    author: 'Дж. К. Роулинг',
    category: 'Фэнтези',
    year: 1997,
    pages: 432,
    description:
      'Первая книга о юном волшебнике и его пути в Хогвартс. История о дружбе, взрослении и силе выбора.',
  },
])

const categories = computed(() =>
  [...new Set(books.value.map((book) => book.category))].sort((left, right) =>
    left.localeCompare(right, 'ru'),
  ),
)

function addBook(bookData) {
  const nextId = books.value.reduce((maxId, book) => Math.max(maxId, book.id), 0) + 1
  const newBook = {
    id: nextId,
    ...bookData,
  }

  books.value = [newBook, ...books.value]
  return newBook
}

provide('library', {
  books,
  categories,
  addBook,
  libraryOwner,
})
</script>

<template>
  <div class="app-shell">
    <header class="site-header">
      <div>
        <RouterLink class="brand" to="/">Читальный зал</RouterLink>
        <p class="brand-meta">Учебный MVP онлайн-библиотеки на Vue.js и Vue Router</p>
      </div>

      <nav class="nav">
        <RouterLink class="nav__link" to="/">Главная</RouterLink>
        <RouterLink class="nav__link" to="/books">Каталог книг</RouterLink>
      </nav>
    </header>

    <main class="page-frame">
      <RouterView />
    </main>

    <footer class="site-footer">
      <p>{{ libraryOwner.name }}</p>
      <p>ИГУ, группа {{ libraryOwner.group }}</p>
      <p>{{ books.length }} книг в каталоге</p>
    </footer>
  </div>
</template>
