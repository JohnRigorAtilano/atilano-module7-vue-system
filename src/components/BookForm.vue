<template>
  <section class="book-form-card">

    <!-- Form Header -->
    <div class="form-header">
      <div class="form-title-wrapper">
        <div class="form-icon">
          <svg
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="1.8"
          >
            <path d="M4 19.5A2.5 2.5 0 0 1 6.5 17H20"></path>
            <path d="M6.5 2H20v20H6.5A2.5 2.5 0 0 1 4 19.5v-15A2.5 2.5 0 0 1 6.5 2Z"></path>
          </svg>
        </div>

        <div>
          <p class="form-eyebrow">
            {{ isEditing ? 'UPDATE RECORD' : 'NEW RECORD' }}
          </p>

          <h2>
            {{ isEditing ? 'Edit Book' : 'Add New Book' }}
          </h2>

          <p>
            {{
              isEditing
                ? 'Update the selected book information.'
                : 'Add a new book to your library collection.'
            }}
          </p>
        </div>
      </div>

      <!-- Close Button -->
      <button
        type="button"
        class="form-close-btn"
        @click="$emit('cancel')"
        title="Close form"
      >
        ×
      </button>
    </div>

    <!-- Validation Message -->
    <transition name="message">
      <div
        v-if="errorMessage"
        class="form-message error-message"
      >
        <span class="message-icon">!</span>
        <span>{{ errorMessage }}</span>
      </div>
    </transition>

    <!-- Book Form -->
    <form
      class="book-form"
      @submit.prevent="handleSubmit"
    >

      <!-- Title -->
      <div class="form-group full-width">
        <label for="title">
          Book Title
          <span>*</span>
        </label>

        <div
          class="input-wrapper"
          :class="{ invalid: errors.title }"
        >
          <svg
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="1.8"
          >
            <path d="M4 19.5A2.5 2.5 0 0 1 6.5 17H20"></path>
            <path d="M6.5 2H20v20H6.5A2.5 2.5 0 0 1 4 19.5v-15A2.5 2.5 0 0 1 6.5 2Z"></path>
          </svg>

          <input
            id="title"
            v-model="form.title"
            type="text"
            placeholder="Enter book title"
            autocomplete="off"
          />
        </div>

        <small v-if="errors.title" class="field-error">
          {{ errors.title }}
        </small>
      </div>

      <!-- Author -->
      <div class="form-group">
        <label for="author">
          Author
          <span>*</span>
        </label>

        <div
          class="input-wrapper"
          :class="{ invalid: errors.author }"
        >
          <svg
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="1.8"
          >
            <circle cx="12" cy="8" r="4"></circle>
            <path d="M5 21a7 7 0 0 1 14 0"></path>
          </svg>

          <input
            id="author"
            v-model="form.author"
            type="text"
            placeholder="Enter author name"
            autocomplete="off"
          />
        </div>

        <small v-if="errors.author" class="field-error">
          {{ errors.author }}
        </small>
      </div>

      <!-- Category -->
      <div class="form-group">
        <label for="category">
          Category
          <span>*</span>
        </label>

        <div
          class="input-wrapper"
          :class="{ invalid: errors.category }"
        >
          <svg
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="1.8"
          >
            <path d="M20 12v8a2 2 0 0 1-2 2H6a2 2 0 0 1-2-2v-8"></path>
            <path d="M4 7h16"></path>
            <path d="M9 7V4a1 1 0 0 1 1-1h4a1 1 0 0 1 1 1v3"></path>
          </svg>

          <select
            id="category"
            v-model="form.category"
          >
            <option value="" disabled>
              Select category
            </option>

            <option value="Fiction">
              Fiction
            </option>

            <option value="Science">
              Science
            </option>

            <option value="Technology">
              Technology
            </option>

            <option value="History">
              History
            </option>

            <option value="Education">
              Education
            </option>

            <option value="Business">
              Business
            </option>

            <option value="Other">
              Other
            </option>
          </select>
        </div>

        <small v-if="errors.category" class="field-error">
          {{ errors.category }}
        </small>
      </div>

      <!-- ISBN -->
      <div class="form-group">
        <label for="isbn">
          ISBN
          <span>*</span>
        </label>

        <div
          class="input-wrapper"
          :class="{ invalid: errors.isbn }"
        >
          <svg
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="1.8"
          >
            <rect x="4" y="3" width="16" height="18" rx="2"></rect>
            <path d="M8 7h8"></path>
            <path d="M8 11h8"></path>
            <path d="M8 15h5"></path>
          </svg>

          <input
            id="isbn"
            v-model="form.isbn"
            type="text"
            placeholder="e.g. 978-1234567890"
            autocomplete="off"
          />
        </div>

        <small v-if="errors.isbn" class="field-error">
          {{ errors.isbn }}
        </small>
      </div>

      <!-- Published Year -->
      <div class="form-group">
        <label for="publishedYear">
          Published Year
          <span>*</span>
        </label>

        <div
          class="input-wrapper"
          :class="{ invalid: errors.publishedYear }"
        >
          <svg
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="1.8"
          >
            <rect x="3" y="4" width="18" height="17" rx="2"></rect>
            <path d="M16 2v4"></path>
            <path d="M8 2v4"></path>
            <path d="M3 10h18"></path>
          </svg>

          <input
            id="publishedYear"
            v-model.number="form.publishedYear"
            type="number"
            min="1000"
            :max="currentYear"
            placeholder="e.g. 2024"
          />
        </div>

        <small
          v-if="errors.publishedYear"
          class="field-error"
        >
          {{ errors.publishedYear }}
        </small>
      </div>

      <!-- Quantity -->
      <div class="form-group">
        <label for="quantity">
          Quantity
          <span>*</span>
        </label>

        <div
          class="input-wrapper"
          :class="{ invalid: errors.quantity }"
        >
          <svg
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="1.8"
          >
            <path d="M4 19V5"></path>
            <path d="M4 5h13l3 3v11H4"></path>
            <path d="M17 5v4h3"></path>
          </svg>

          <input
            id="quantity"
            v-model.number="form.quantity"
            type="number"
            min="0"
            placeholder="Enter quantity"
          />
        </div>

        <small v-if="errors.quantity" class="field-error">
          {{ errors.quantity }}
        </small>
      </div>

      <!-- Status -->
      <div class="form-group">
        <label for="status">
          Status
          <span>*</span>
        </label>

        <div class="input-wrapper">
          <svg
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="1.8"
          >
            <circle cx="12" cy="12" r="9"></circle>
            <path d="m8 12 2.5 2.5L16 9"></path>
          </svg>

          <select
            id="status"
            v-model="form.status"
          >
            <option value="Available">
              Available
            </option>

            <option value="Borrowed">
              Borrowed
            </option>

            <option value="Reserved">
              Reserved
            </option>

            <option value="Unavailable">
              Unavailable
            </option>
          </select>
        </div>
      </div>

      <!-- Form Actions -->
      <div class="form-actions full-width">

        <button
          v-if="isEditing"
          type="button"
          class="secondary-btn"
          @click="$emit('cancel')"
        >
          Cancel
        </button>

        <button
          type="submit"
          class="primary-btn"
          :disabled="isSubmitting"
        >
          <span v-if="isSubmitting" class="loading-spinner"></span>

          <svg
            v-else
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
          >
            <path d="M12 5v14"></path>
            <path d="M5 12h14"></path>
          </svg>

          {{ isSubmitting ? 'Saving...' : isEditing ? 'Update Book' : 'Add Book' }}
        </button>
      </div>

    </form>
  </section>
</template>

<script setup>
import { computed, reactive, ref, watch } from 'vue'

const props = defineProps({
  book: {
    type: Object,
    default: null
  },

  isEditing: {
    type: Boolean,
    default: false
  }
})

const emit = defineEmits([
  'submit',
  'cancel'
])

const currentYear = new Date().getFullYear()

const isSubmitting = ref(false)

const errorMessage = ref('')

const form = reactive({
  title: '',
  author: '',
  category: '',
  isbn: '',
  publishedYear: '',
  quantity: 1,
  status: 'Available'
})

const errors = reactive({
  title: '',
  author: '',
  category: '',
  isbn: '',
  publishedYear: '',
  quantity: ''
})

function clearErrors() {
  Object.keys(errors).forEach((key) => {
    errors[key] = ''
  })

  errorMessage.value = ''
}

function loadBook(book) {
  clearErrors()

  if (!book) {
    form.title = ''
    form.author = ''
    form.category = ''
    form.isbn = ''
    form.publishedYear = ''
    form.quantity = 1
    form.status = 'Available'

    return
  }

  form.title = book.title ?? ''
  form.author = book.author ?? ''
  form.category = book.category ?? ''
  form.isbn = book.isbn ?? ''
  form.publishedYear = book.publishedYear ?? ''
  form.quantity = book.quantity ?? 1
  form.status = book.status ?? 'Available'
}

watch(
  () => props.book,
  (newBook) => {
    loadBook(newBook)
  },
  {
    immediate: true
  }
)

function validateForm() {
  clearErrors()

  let valid = true

  if (!form.title.trim()) {
    errors.title = 'Book title is required.'
    valid = false
  }

  if (!form.author.trim()) {
    errors.author = 'Author name is required.'
    valid = false
  }

  if (!form.category) {
    errors.category = 'Please select a category.'
    valid = false
  }

  if (!form.isbn.trim()) {
    errors.isbn = 'ISBN is required.'
    valid = false
  }

  const year = Number(form.publishedYear)

  if (!year) {
    errors.publishedYear = 'Published year is required.'
    valid = false
  } else if (year < 1000 || year > currentYear) {
    errors.publishedYear =
      `Year must be between 1000 and ${currentYear}.`
    valid = false
  }

  const quantity = Number(form.quantity)

  if (form.quantity === '' || form.quantity === null) {
    errors.quantity = 'Quantity is required.'
    valid = false
  } else if (quantity < 0) {
    errors.quantity = 'Quantity cannot be negative.'
    valid = false
  }

  if (!valid) {
    errorMessage.value =
      'Please complete all required fields correctly.'
  }

  return valid
}

async function handleSubmit() {
  if (!validateForm()) {
    return
  }

  isSubmitting.value = true

  const cleanRecord = {
    title: form.title.trim(),
    author: form.author.trim(),
    category: form.category,
    isbn: form.isbn.trim(),
    publishedYear: Number(form.publishedYear),
    quantity: Number(form.quantity),
    status: form.status
  }

  try {
    await new Promise((resolve) => {
      setTimeout(resolve, 250)
    })

    emit('submit', cleanRecord)
  } finally {
    isSubmitting.value = false
  }
}
</script>