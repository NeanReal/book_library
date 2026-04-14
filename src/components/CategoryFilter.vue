<script setup>
defineProps({
  categories: {
    type: Array,
    default: () => [],
  },
  selectedCategory: {
    type: String,
    default: '',
  },
})

const emit = defineEmits(['select'])
</script>

<template>
  <div class="filter-panel">
    <div class="filter-header">
      <div>
        <p class="eyebrow">Фильтрация</p>
        <h2>Категории</h2>
      </div>
      <span class="filter-current">
        {{ selectedCategory ? `Активна: ${selectedCategory}` : 'Показаны все книги' }}
      </span>
    </div>

    <div class="filter-list">
      <button
        class="filter-button"
        :class="{ 'filter-button--active': !selectedCategory }"
        type="button"
        @click="emit('select', '')"
      >
        Все книги
      </button>

      <button
        v-for="category in categories"
        :key="category"
        class="filter-button"
        :class="{ 'filter-button--active': selectedCategory === category }"
        type="button"
        @click="emit('select', category)"
      >
        {{ category }}
      </button>
    </div>
  </div>
</template>
