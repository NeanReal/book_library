<script setup>
import { computed, inject } from 'vue'
import { useRoute, useRouter } from 'vue-router'
import AddBook from '../components/AddBook.vue'
import BookList from '../components/BookList.vue'
import CategoryFilter from '../components/CategoryFilter.vue'

const library = inject('library')

if (!library) {
  throw new Error('Library provider is missing')
}

const route = useRoute()
const router = useRouter()

const selectedCategory = computed(() =>
  typeof route.params.category === 'string' ? route.params.category : '',
)

const filteredBooks = computed(() => {
  if (!selectedCategory.value) {
    return library.books.value
  }

  return library.books.value.filter((book) => book.category === selectedCategory.value)
})

function handleSelectCategory(category) {
  if (category) {
    router.push({
      name: 'books-by-category',
      params: { category },
    })
    return
  }

  router.push({ name: 'books' })
}

function handleAddBook(bookData) {
  const newBook = library.addBook(bookData)

  router.push({
    name: 'book-detail',
    params: { id: newBook.id },
  })
}
</script>

<template>
  <section class="page stack">
    <div class="page-intro">
      <div>
        <p class="eyebrow">Каталог</p>
        <h1>Список книг</h1>
      </div>
      <p class="lead lead--compact">
        {{ selectedCategory ? `Сейчас открыт раздел «${selectedCategory}».` : 'Показаны все книги библиотеки.' }}
      </p>
    </div>

    <CategoryFilter
      :categories="library.categories.value"
      :selected-category="selectedCategory"
      @select="handleSelectCategory"
    />

    <div class="catalog-layout">
      <div class="stack">
        <article class="section-card">
          <div class="list-header">
            <h2>Каталог книг</h2>
            <span class="count-pill">{{ filteredBooks.length }}</span>
          </div>

          <BookList :books="filteredBooks" />
        </article>
      </div>

      <aside class="sidebar-card">
        <p class="eyebrow">Новая запись</p>
        <h2>Добавить книгу</h2>
        <p class="lead lead--compact">
          После добавления откроется отдельная страница книги с динамическим маршрутом.
        </p>
        <AddBook @submit="handleAddBook" />
      </aside>
    </div>
  </section>
</template>
