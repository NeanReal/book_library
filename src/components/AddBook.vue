<script setup>
import { computed, reactive } from 'vue'

const emit = defineEmits(['submit'])

const form = reactive({
  title: '',
  author: '',
  category: '',
  year: '',
  pages: '',
  description: '',
})

const isFormValid = computed(
  () =>
    Boolean(
    form.title.trim() &&
    form.author.trim() &&
    form.category.trim() &&
    Number(form.year) > 0 &&
    Number(form.pages) > 0 &&
      form.description.trim(),
    ),
)

function resetForm() {
  form.title = ''
  form.author = ''
  form.category = ''
  form.year = ''
  form.pages = ''
  form.description = ''
}

function handleSubmit() {
  if (!isFormValid.value) {
    return
  }

  emit('submit', {
    title: form.title.trim(),
    author: form.author.trim(),
    category: form.category.trim(),
    year: Number(form.year),
    pages: Number(form.pages),
    description: form.description.trim(),
  })

  resetForm()
}
</script>

<template>
  <form class="form" @submit.prevent="handleSubmit">
    <label class="field">
      <span>Название</span>
      <input v-model="form.title" class="input" type="text" placeholder="Например, Идиот" />
    </label>

    <label class="field">
      <span>Автор</span>
      <input
        v-model="form.author"
        class="input"
        type="text"
        placeholder="Например, Фёдор Достоевский"
      />
    </label>

    <label class="field">
      <span>Категория</span>
      <input
        v-model="form.category"
        class="input"
        type="text"
        placeholder="Классика, Наука, Фэнтези..."
      />
    </label>

    <div class="form-grid">
      <label class="field">
        <span>Год</span>
        <input v-model="form.year" class="input" type="number" min="1" placeholder="2024" />
      </label>

      <label class="field">
        <span>Страницы</span>
        <input v-model="form.pages" class="input" type="number" min="1" placeholder="320" />
      </label>
    </div>

    <label class="field">
      <span>Описание</span>
      <textarea
        v-model="form.description"
        class="input textarea"
        rows="4"
        placeholder="Кратко опишите содержание книги"
      />
    </label>

    <button class="button" type="submit" :disabled="!isFormValid">Добавить книгу</button>
  </form>
</template>
