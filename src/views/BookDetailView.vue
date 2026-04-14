<script setup>
import { computed, inject } from 'vue'
import { useRoute } from 'vue-router'
import BookDetail from '../components/BookDetail.vue'

const library = inject('library')

if (!library) {
  throw new Error('Library provider is missing')
}

const route = useRoute()

const book = computed(() => {
  const id = Number(route.params.id)
  return library.books.value.find((item) => item.id === id) ?? null
})
</script>

<template>
  <section class="page">
    <BookDetail :book="book" />
  </section>
</template>
