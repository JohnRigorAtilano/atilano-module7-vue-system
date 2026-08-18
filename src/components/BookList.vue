<template>
  <section class="book-list-card">

    <!-- List Header -->
    <div class="list-header">
      <div>
        <p class="section-eyebrow">LIBRARY COLLECTION</p>

        <h2>Books</h2>

        <p class="section-description">
          Manage and monitor your library collection.
        </p>
      </div>

      <div class="list-summary">
        <div class="summary-number">
          {{ books.length }}
        </div>

        <div class="summary-label">
          {{ books.length === 1 ? 'Book' : 'Books' }}
        </div>
      </div>
    </div>

    <!-- Search / Filter Bar -->
    <div class="book-toolbar">

      <div class="book-search">
        <svg
          viewBox="0 0 24 24"
          fill="none"
          stroke="currentColor"
          stroke-width="2"
        >
          <circle cx="11" cy="11" r="7"></circle>
          <path d="m20 20-4-4"></path>
        </svg>

        <input
          v-model="localSearch"
          type="text"
          placeholder="Search by title or author..."
        />

        <button
          v-if="localSearch"
          type="button"
          class="clear-search"
          @click="localSearch = ''"
          title="Clear search"
        >
          ×
        </button>
      </div>

      <!-- Result Count -->
      <div class="result-count">
        Showing
        <strong>{{ filteredBooks.length }}</strong>
        of
        <strong>{{ books.length }}</strong>
      </div>
    </div>

    <!-- Desktop Table -->
    <div
      v-if="filteredBooks.length"
      class="table-wrapper"
    >
      <table class="books-table">

        <thead>
          <tr>
            <th>BOOK</th>
            <th>AUTHOR</th>
            <th>CATEGORY</th>
            <th>ISBN</th>
            <th>YEAR</th>
            <th>QTY</th>
            <th>STATUS</th>
            <th>ACTIONS</th>
          </tr>
        </thead>

        <tbody>
          <tr
            v-for="book in filteredBooks"
            :key="book.id"
          >

            <!-- Book -->
            <td>
              <div class="book-cell">

                <div
                  class="book-cover"
                  :class="getCategoryClass(book.category)"
                >
                  <span>
                    {{ getInitials(book.title) }}
                  </span>
                </div>

                <div class="book-info">
                  <strong>
                    {{ book.title }}
                  </strong>

                  <span>
                    ID #{{ book.id }}
                  </span>
                </div>

              </div>
            </td>

            <!-- Author -->
            <td>
              <span class="author-name">
                {{ book.author }}
              </span>
            </td>

            <!-- Category -->
            <td>
              <span class="category-tag">
                {{ book.category }}
              </span>
            </td>

            <!-- ISBN -->
            <td>
              <span class="isbn-text">
                {{ book.isbn }}
              </span>
            </td>

            <!-- Year -->
            <td>
              {{ book.publishedYear }}
            </td>

            <!-- Quantity -->
            <td>
              <span
                class="quantity-value"
                :class="{
                  'low-stock': Number(book.quantity) <= 2
                }"
              >
                {{ book.quantity }}
              </span>
            </td>

            <!-- Status -->
            <td>
              <span
                class="status-badge"
                :class="getStatusClass(book.status)"
              >
                <span class="status-dot"></span>
                {{ book.status }}
              </span>
            </td>

            <!-- Actions -->
            <td>
              <div class="table-actions">

                <button
                  type="button"
                  class="action-btn edit-action"
                  title="Edit book"
                  @click="$emit('edit', book)"
                >
                  <svg
                    viewBox="0 0 24 24"
                    fill="none"
                    stroke="currentColor"
                    stroke-width="2"
                  >
                    <path
                      d="M12 20h9"
                    ></path>

                    <path
                      d="M16.5 3.5a2.12 2.12 0 0 1 3 3L8 18l-4 1 1-4Z"
                    ></path>
                  </svg>
                </button>

                <button
                  type="button"
                  class="action-btn delete-action"
                  title="Delete book"
                  @click="$emit('delete', book)"
                >
                  <svg
                    viewBox="0 0 24 24"
                    fill="none"
                    stroke="currentColor"
                    stroke-width="2"
                  >
                    <path d="M3 6h18"></path>
                    <path d="M8 6V4h8v2"></path>
                    <path d="M19 6l-1 15H6L5 6"></path>
                    <path d="M10 11v6"></path>
                    <path d="M14 11v6"></path>
                  </svg>
                </button>

              </div>
            </td>

          </tr>
        </tbody>
      </table>
    </div>

    <!-- Empty State -->
    <div
      v-else
      class="empty-state"
    >

      <div class="empty-icon">
        <svg
          viewBox="0 0 24 24"
          fill="none"
          stroke="currentColor"
          stroke-width="1.6"
        >
          <path d="M4 19.5A2.5 2.5 0 0 1 6.5 17H20"></path>
          <path d="M6.5 2H20v20H6.5A2.5 2.5 0 0 1 4 19.5v-15A2.5 2.5 0 0 1 6.5 2Z"></path>
        </svg>
      </div>

      <h3>
        {{ localSearch ? 'No books found' : 'Your library is empty' }}
      </h3>

      <p>
        {{
          localSearch
            ? 'Try searching for another title or author.'
            : 'Add your first book to start building your collection.'
        }}
      </p>

      <button
        v-if="localSearch"
        type="button"
        class="empty-clear-btn"
        @click="localSearch = ''"
      >
        Clear Search
      </button>

      <button
        v-else
        type="button"
        class="empty-add-btn"
        @click="$emit('add')"
      >
        <svg
          viewBox="0 0 24 24"
          fill="none"
          stroke="currentColor"
          stroke-width="2"
        >
          <path d="M12 5v14"></path>
          <path d="M5 12h14"></path>
        </svg>

        Add Your First Book
      </button>

    </div>

  </section>
</template>

<script setup>
import { computed, ref } from 'vue'

const props = defineProps({
  books: {
    type: Array,
    default: () => []
  }
})

defineEmits([
  'edit',
  'delete',
  'add'
])

const localSearch = ref('')

const filteredBooks = computed(() => {
  const keyword = localSearch.value
    .toLowerCase()
    .trim()

  if (!keyword) {
    return props.books
  }

  return props.books.filter((book) => {
    const title = String(book.title ?? '').toLowerCase()
    const author = String(book.author ?? '').toLowerCase()
    const category = String(book.category ?? '').toLowerCase()
    const isbn = String(book.isbn ?? '').toLowerCase()

    return (
      title.includes(keyword) ||
      author.includes(keyword) ||
      category.includes(keyword) ||
      isbn.includes(keyword)
    )
  })
})

function getInitials(title) {
  if (!title) {
    return 'BK'
  }

  const words = title
    .trim()
    .split(/\s+/)
    .filter(Boolean)

  if (words.length === 1) {
    return words[0].substring(0, 2).toUpperCase()
  }

  return (
    words[0][0] +
    words[1][0]
  ).toUpperCase()
}

function getCategoryClass(category) {
  const normalized = String(category ?? '')
    .toLowerCase()

  if (normalized === 'fiction') {
    return 'cover-purple'
  }

  if (normalized === 'science') {
    return 'cover-blue'
  }

  if (normalized === 'technology') {
    return 'cover-cyan'
  }

  if (normalized === 'history') {
    return 'cover-orange'
  }

  if (normalized === 'education') {
    return 'cover-green'
  }

  if (normalized === 'business') {
    return 'cover-pink'
  }

  return 'cover-default'
}

function getStatusClass(status) {
  const normalized = String(status ?? '')
    .toLowerCase()

  if (normalized === 'available') {
    return 'status-available'
  }

  if (normalized === 'borrowed') {
    return 'status-borrowed'
  }

  if (normalized === 'reserved') {
    return 'status-reserved'
  }

  if (normalized === 'unavailable') {
    return 'status-unavailable'
  }

  return 'status-default'
}
</script>