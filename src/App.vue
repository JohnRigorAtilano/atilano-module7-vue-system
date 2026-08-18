<template>
  <div
    class="app-shell"
    :class="{ 'light-mode': isLightMode }"
  >
    <!-- Mobile Overlay -->
    <div
      v-if="mobileMenuOpen"
      class="mobile-overlay"
      @click="mobileMenuOpen = false"
    ></div>

    <!-- Sidebar -->
    <aside
      class="sidebar"
      :class="{ 'sidebar-open': mobileMenuOpen }"
    >
      <div class="sidebar-brand">
        <div class="brand-icon">
          <svg
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="1.8"
          >
            <path d="M4 19.5A2.5 2.5 0 0 1 6.5 17H20"></path>
            <path
              d="M6.5 2H20v20H6.5A2.5 2.5 0 0 1 4 19.5v-15A2.5 2.5 0 0 1 6.5 2Z"
            ></path>
          </svg>
        </div>

        <div class="brand-text">
          <strong>LIBRA</strong>
          <span>Library System</span>
        </div>

        <button
          type="button"
          class="sidebar-close"
          @click="mobileMenuOpen = false"
        >
          ×
        </button>
      </div>

      <nav class="sidebar-nav">

        <p class="nav-label">MAIN MENU</p>

        <button
          type="button"
          class="nav-item active"
          @click="scrollToTop"
        >
          <svg
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="1.8"
          >
            <rect x="3" y="3" width="7" height="7" rx="1"></rect>
            <rect x="14" y="3" width="7" height="7" rx="1"></rect>
            <rect x="3" y="14" width="7" height="7" rx="1"></rect>
            <rect x="14" y="14" width="7" height="7" rx="1"></rect>
          </svg>

          <span>Dashboard</span>
        </button>

        <button
          type="button"
          class="nav-item"
          @click="focusBookList"
        >
          <svg
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="1.8"
          >
            <path d="M4 19.5A2.5 2.5 0 0 1 6.5 17H20"></path>
            <path
              d="M6.5 2H20v20H6.5A2.5 2.5 0 0 1 4 19.5v-15A2.5 2.5 0 0 1 6.5 2Z"
            ></path>
          </svg>

          <span>Books</span>

          <span class="nav-count">
            {{ books.length }}
          </span>
        </button>

        <button
          type="button"
          class="nav-item"
          @click="openAddBook"
        >
          <svg
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="1.8"
          >
            <circle cx="12" cy="12" r="9"></circle>
            <path d="M12 8v8"></path>
            <path d="M8 12h8"></path>
          </svg>

          <span>Add Book</span>
        </button>

        <p class="nav-label secondary-label">MANAGEMENT</p>

        <button
          type="button"
          class="nav-item"
          @click="showComingSoon('Borrowing Management')"
        >
          <svg
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="1.8"
          >
            <path d="M3 6h18"></path>
            <path d="M6 6v12"></path>
            <path d="M18 6v12"></path>
            <path d="M4 18h16"></path>
            <path d="M8 10h8"></path>
            <path d="M8 14h5"></path>
          </svg>

          <span>Borrowing</span>
        </button>

        <button
          type="button"
          class="nav-item"
          @click="showComingSoon('Members Management')"
        >
          <svg
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="1.8"
          >
            <circle cx="9" cy="8" r="3"></circle>
            <path d="M3 21a6 6 0 0 1 12 0"></path>
            <path d="M16 11a3 3 0 0 0 0-6"></path>
            <path d="M18 14a5 5 0 0 1 3 7"></path>
          </svg>

          <span>Members</span>
        </button>

        <button
          type="button"
          class="nav-item"
          @click="showComingSoon('Reports')"
        >
          <svg
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="1.8"
          >
            <path d="M4 20V10"></path>
            <path d="M10 20V4"></path>
            <path d="M16 20v-7"></path>
            <path d="M22 20V7"></path>
          </svg>

          <span>Reports</span>
        </button>

      </nav>

      <!-- Sidebar Bottom -->
      <div class="sidebar-bottom">

        <div class="system-status">
          <span class="pulse-dot"></span>

          <div>
            <strong>System Online</strong>
            <span>Local storage active</span>
          </div>
        </div>

        <div class="sidebar-user">
          <div class="sidebar-avatar">
            JR
          </div>

          <div>
            <strong>John Rigor P. Atilano</strong>
            <span>BSCS 3A</span>
          </div>
        </div>

      </div>
    </aside>

    <!-- Main Content -->
    <main class="main-content">

      <AppHeader
        title="Library Dashboard"
        subtitle="Manage your books and library collection"
        :search-term="globalSearch"
        @toggle-menu="mobileMenuOpen = !mobileMenuOpen"
        @toggle-theme="toggleTheme"
        @update:search-term="handleGlobalSearch"
      />

      <div class="dashboard-content">

        <!-- Welcome Banner -->
        <section class="welcome-banner">

          <div class="welcome-content">
            <p class="welcome-eyebrow">
              LIBRARY MANAGEMENT SYSTEM
            </p>

            <h2>
              Welcome back, John Rigor
              <span>👋</span>
            </h2>

            <p>
              Keep your library organized and your collection
              always within reach.
            </p>

            <button
              type="button"
              class="welcome-action"
              @click="openAddBook"
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

              Add New Book
            </button>
          </div>

          <div class="welcome-visual">

            <div class="floating-book book-one">
              <svg
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="1.6"
              >
                <path d="M4 19.5A2.5 2.5 0 0 1 6.5 17H20"></path>
                <path
                  d="M6.5 2H20v20H6.5A2.5 2.5 0 0 1 4 19.5v-15A2.5 2.5 0 0 1 6.5 2Z"
                ></path>
              </svg>
            </div>

            <div class="floating-book book-two">
              <svg
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="1.6"
              >
                <path d="M4 19.5A2.5 2.5 0 0 1 6.5 17H20"></path>
                <path
                  d="M6.5 2H20v20H6.5A2.5 2.5 0 0 1 4 19.5v-15A2.5 2.5 0 0 1 6.5 2Z"
                ></path>
              </svg>
            </div>

            <div class="floating-circle"></div>

          </div>

        </section>

        <!-- Statistics -->
        <section class="stats-grid">

          <article class="stat-card">
            <div class="stat-icon purple">
              <svg
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="1.8"
              >
                <path d="M4 19.5A2.5 2.5 0 0 1 6.5 17H20"></path>
                <path
                  d="M6.5 2H20v20H6.5A2.5 2.5 0 0 1 4 19.5v-15A2.5 2.5 0 0 1 6.5 2Z"
                ></path>
              </svg>
            </div>

            <div class="stat-content">
              <span>Total Books</span>
              <strong>{{ statistics.totalBooks }}</strong>
              <small>In collection</small>
            </div>

            <div class="stat-trend">
              <span>LIVE</span>
            </div>
          </article>

          <article class="stat-card">
            <div class="stat-icon cyan">
              <svg
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="1.8"
              >
                <circle cx="12" cy="12" r="9"></circle>
                <path d="m8 12 2.5 2.5L16 9"></path>
              </svg>
            </div>

            <div class="stat-content">
              <span>Available</span>
              <strong>{{ statistics.available }}</strong>
              <small>Ready to borrow</small>
            </div>

            <div class="stat-trend positive">
              {{ availabilityRate }}%
            </div>
          </article>

          <article class="stat-card">
            <div class="stat-icon orange">
              <svg
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="1.8"
              >
                <path d="M3 6h18"></path>
                <path d="M6 6v12"></path>
                <path d="M18 6v12"></path>
                <path d="M4 18h16"></path>
              </svg>
            </div>

            <div class="stat-content">
              <span>Borrowed</span>
              <strong>{{ statistics.borrowed }}</strong>
              <small>Currently borrowed</small>
            </div>

            <div class="stat-trend">
              ACTIVE
            </div>
          </article>

          <article class="stat-card">
            <div class="stat-icon pink">
              <svg
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="1.8"
              >
                <path d="M4 20V10"></path>
                <path d="M10 20V4"></path>
                <path d="M16 20v-7"></path>
                <path d="M22 20V7"></path>
              </svg>
            </div>

            <div class="stat-content">
              <span>Total Copies</span>
              <strong>{{ statistics.totalCopies }}</strong>
              <small>Physical copies</small>
            </div>

            <div class="stat-trend">
              STOCK
            </div>
          </article>

        </section>

        <!-- Main Grid -->
        <section class="content-grid">

          <!-- Books -->
          <div
            id="book-list-section"
            class="books-section"
          >
            <BookList
              :books="filteredBooks"
              @edit="startEdit"
              @delete="confirmDelete"
              @add="openAddBook"
            />
          </div>

          <!-- Right Panel -->
          <aside class="dashboard-side-panel">

            <!-- Quick Actions -->
            <div class="side-card quick-actions-card">

              <div class="side-card-header">
                <div>
                  <p class="section-eyebrow">
                    SHORTCUTS
                  </p>

                  <h3>Quick Actions</h3>
                </div>

                <span class="side-card-icon">
                  ⚡
                </span>
              </div>

              <button
                type="button"
                class="quick-action"
                @click="openAddBook"
              >
                <span class="quick-icon purple-bg">
                  +
                </span>

                <span>
                  <strong>Add New Book</strong>
                  <small>Create a new record</small>
                </span>

                <span class="arrow">→</span>
              </button>

              <button
                type="button"
                class="quick-action"
                @click="focusBookList"
              >
                <span class="quick-icon cyan-bg">
                  ◉
                </span>

                <span>
                  <strong>View Collection</strong>
                  <small>Browse all books</small>
                </span>

                <span class="arrow">→</span>
              </button>

              <button
                type="button"
                class="quick-action"
                @click="exportBooks"
              >
                <span class="quick-icon orange-bg">
                  ↓
                </span>

                <span>
                  <strong>Export Records</strong>
                  <small>Download JSON backup</small>
                </span>

                <span class="arrow">→</span>
              </button>

            </div>

            <!-- Category Overview -->
            <div class="side-card category-card">

              <div class="side-card-header">
                <div>
                  <p class="section-eyebrow">
                    ANALYTICS
                  </p>

                  <h3>Categories</h3>
                </div>
              </div>

              <div
                v-if="categoryStats.length"
                class="category-list"
              >
                <div
                  v-for="category in categoryStats"
                  :key="category.name"
                  class="category-row"
                >
                  <div class="category-info">
                    <span
                      class="category-dot"
                      :class="category.color"
                    ></span>

                    <span>
                      {{ category.name }}
                    </span>
                  </div>

                  <strong>
                    {{ category.count }}
                  </strong>
                </div>
              </div>

              <div
                v-else
                class="mini-empty"
              >
                No category data yet.
              </div>

            </div>

            <!-- Recent Activity -->
            <div class="side-card activity-card">

              <div class="side-card-header">
                <div>
                  <p class="section-eyebrow">
                    ACTIVITY
                  </p>

                  <h3>Recent Books</h3>
                </div>
              </div>

              <div
                v-if="recentBooks.length"
                class="activity-list"
              >
                <div
                  v-for="book in recentBooks"
                  :key="book.id"
                  class="activity-item"
                >
                  <div class="activity-avatar">
                    {{ getInitials(book.title) }}
                  </div>

                  <div class="activity-info">
                    <strong>
                      {{ book.title }}
                    </strong>

                    <span>
                      {{ book.author }}
                    </span>
                  </div>

                  <span
                    class="activity-status"
                    :class="getStatusClass(book.status)"
                  >
                    {{ book.status }}
                  </span>
                </div>
              </div>

              <div
                v-else
                class="mini-empty"
              >
                No recent books.
              </div>

            </div>

          </aside>

        </section>

        <!-- Book Form Modal -->
        <transition name="modal">
          <div
            v-if="showForm"
            class="modal-backdrop"
            @click.self="closeForm"
          >
            <div class="modal-container">

              <BookForm
                :book="editingBook"
                :is-editing="Boolean(editingBook)"
                @submit="saveBook"
                @cancel="closeForm"
              />

            </div>
          </div>
        </transition>

        <!-- Delete Confirmation -->
        <transition name="modal">
          <div
            v-if="bookToDelete"
            class="modal-backdrop"
            @click.self="cancelDelete"
          >
            <div class="confirm-modal">

              <div class="confirm-icon">
                <svg
                  viewBox="0 0 24 24"
                  fill="none"
                  stroke="currentColor"
                  stroke-width="1.8"
                >
                  <path
                    d="M10.3 3.8 2.5 17a2 2 0 0 0 1.7 3h15.6a2 2 0 0 0 1.7-3L13.7 3.8a2 2 0 0 0-3.4 0Z"
                  ></path>
                  <path d="M12 9v4"></path>
                  <path d="M12 17h.01"></path>
                </svg>
              </div>

              <p class="confirm-eyebrow">
                DELETE RECORD
              </p>

              <h3>
                Delete this book?
              </h3>

              <p>
                Are you sure you want to remove
                <strong>
                  "{{ bookToDelete.title }}"
                </strong>
                from your library?
              </p>

              <div class="confirm-actions">

                <button
                  type="button"
                  class="secondary-btn"
                  @click="cancelDelete"
                >
                  Cancel
                </button>

                <button
                  type="button"
                  class="danger-btn"
                  @click="deleteBook"
                >
                  Delete Book
                </button>

              </div>

            </div>
          </div>
        </transition>

        <!-- Toast -->
        <transition name="toast">
          <div
            v-if="toast.show"
            class="toast-message"
            :class="`toast-${toast.type}`"
          >
            <span class="toast-icon">
              {{ toast.type === 'success' ? '✓' : '!' }}
            </span>

            <div>
              <strong>
                {{ toast.title }}
              </strong>

              <span>
                {{ toast.message }}
              </span>
            </div>

            <button
              type="button"
              @click="hideToast"
            >
              ×
            </button>
          </div>
        </transition>

      </div>

      <AppFooter />

    </main>
  </div>
</template>

<script setup>
import {
  computed,
  onMounted,
  ref,
  watch
} from 'vue'

import AppHeader from './components/AppHeader.vue'
import AppFooter from './components/AppFooter.vue'
import BookForm from './components/BookForm.vue'
import BookList from './components/BookList.vue'

const STORAGE_KEY = 'atilano-module7-books'

const books = ref([])

const showForm = ref(false)

const editingBook = ref(null)

const bookToDelete = ref(null)

const mobileMenuOpen = ref(false)

const isLightMode = ref(false)

const globalSearch = ref('')

const toast = ref({
  show: false,
  type: 'success',
  title: '',
  message: ''
})

let toastTimer = null

/* --------------------------------
   LOCAL STORAGE
-------------------------------- */

function loadBooks() {
  try {
    const savedBooks = localStorage.getItem(STORAGE_KEY)

    books.value = savedBooks
      ? JSON.parse(savedBooks)
      : []
  } catch (error) {
    console.error('Unable to load books:', error)

    books.value = []

    showToast(
      'error',
      'Storage Error',
      'Saved records could not be loaded.'
    )
  }
}

function saveBooks() {
  try {
    localStorage.setItem(
      STORAGE_KEY,
      JSON.stringify(books.value)
    )
  } catch (error) {
    console.error('Unable to save books:', error)

    showToast(
      'error',
      'Storage Error',
      'Records could not be saved.'
    )
  }
}

onMounted(() => {
  loadBooks()

  const savedTheme =
    localStorage.getItem('atilano-module7-theme')

  isLightMode.value =
    savedTheme === 'light'
})

watch(
  books,
  () => {
    saveBooks()
  },
  {
    deep: true
  }
)

watch(
  isLightMode,
  (value) => {
    localStorage.setItem(
      'atilano-module7-theme',
      value ? 'light' : 'dark'
    )
  }
)

/* --------------------------------
   FILTERING
-------------------------------- */

const filteredBooks = computed(() => {
  const keyword = globalSearch.value
    .toLowerCase()
    .trim()

  if (!keyword) {
    return books.value
  }

  return books.value.filter((book) => {
    return (
      String(book.title ?? '')
        .toLowerCase()
        .includes(keyword) ||

      String(book.author ?? '')
        .toLowerCase()
        .includes(keyword) ||

      String(book.category ?? '')
        .toLowerCase()
        .includes(keyword) ||

      String(book.isbn ?? '')
        .toLowerCase()
        .includes(keyword)
    )
  })
})

/* --------------------------------
   STATISTICS
-------------------------------- */

const statistics = computed(() => {
  const totalBooks = books.value.length

  const available = books.value.filter(
    (book) => book.status === 'Available'
  ).length

  const borrowed = books.value.filter(
    (book) => book.status === 'Borrowed'
  ).length

  const totalCopies = books.value.reduce(
    (total, book) => {
      return total + Number(book.quantity || 0)
    },
    0
  )

  return {
    totalBooks,
    available,
    borrowed,
    totalCopies
  }
})

const availabilityRate = computed(() => {
  if (!statistics.value.totalBooks) {
    return 0
  }

  return Math.round(
    (statistics.value.available /
      statistics.value.totalBooks) *
      100
  )
})

/* --------------------------------
   CATEGORY ANALYTICS
-------------------------------- */

const categoryColors = [
  'purple-dot',
  'cyan-dot',
  'orange-dot',
  'pink-dot',
  'green-dot',
  'blue-dot'
]

const categoryStats = computed(() => {
  const counts = {}

  books.value.forEach((book) => {
    const category =
      book.category || 'Other'

    counts[category] =
      (counts[category] || 0) + 1
  })

  return Object.entries(counts)
    .sort((a, b) => b[1] - a[1])
    .slice(0, 6)
    .map(([name, count], index) => ({
      name,
      count,
      color:
        categoryColors[
          index % categoryColors.length
        ]
    }))
})

/* --------------------------------
   RECENT BOOKS
-------------------------------- */

const recentBooks = computed(() => {
  return [...books.value]
    .sort(
      (a, b) =>
        Number(b.createdAt || b.id || 0) -
        Number(a.createdAt || a.id || 0)
    )
    .slice(0, 4)
})

/* --------------------------------
   ADD BOOK
-------------------------------- */

function openAddBook() {
  editingBook.value = null
  showForm.value = true
  mobileMenuOpen.value = false
}

function closeForm() {
  showForm.value = false
  editingBook.value = null
}

/* --------------------------------
   CREATE / UPDATE
-------------------------------- */

function saveBook(bookData) {
  if (editingBook.value) {
    const index = books.value.findIndex(
      (book) =>
        book.id === editingBook.value.id
    )

    if (index !== -1) {
      books.value[index] = {
        ...books.value[index],
        ...bookData,
        updatedAt: Date.now()
      }

      showToast(
        'success',
        'Book Updated',
        `"${bookData.title}" was updated successfully.`
      )
    }
  } else {
    const newBook = {
      id: Date.now(),
      ...bookData,
      createdAt: Date.now(),
      updatedAt: Date.now()
    }

    books.value.unshift(newBook)

    showToast(
      'success',
      'Book Added',
      `"${bookData.title}" was added to the library.`
    )
  }

  closeForm()
}

/* --------------------------------
   EDIT
-------------------------------- */

function startEdit(book) {
  editingBook.value = {
    ...book
  }

  showForm.value = true
}

/* --------------------------------
   DELETE
-------------------------------- */

function confirmDelete(book) {
  bookToDelete.value = book
}

function cancelDelete() {
  bookToDelete.value = null
}

function deleteBook() {
  if (!bookToDelete.value) {
    return
  }

  const title =
    bookToDelete.value.title

  books.value = books.value.filter(
    (book) =>
      book.id !== bookToDelete.value.id
  )

  bookToDelete.value = null

  showToast(
    'success',
    'Book Deleted',
    `"${title}" was removed from the library.`
  )
}

/* --------------------------------
   SEARCH
-------------------------------- */

function handleGlobalSearch(value) {
  globalSearch.value = value

  if (value) {
    setTimeout(() => {
      focusBookList()
    }, 50)
  }
}

/* --------------------------------
   NAVIGATION
-------------------------------- */

function scrollToTop() {
  window.scrollTo({
    top: 0,
    behavior: 'smooth'
  })

  mobileMenuOpen.value = false
}

function focusBookList() {
  const section =
    document.getElementById(
      'book-list-section'
    )

  if (section) {
    section.scrollIntoView({
      behavior: 'smooth',
      block: 'start'
    })
  }

  mobileMenuOpen.value = false
}

/* --------------------------------
   THEME
-------------------------------- */

function toggleTheme() {
  isLightMode.value =
    !isLightMode.value

  showToast(
    'success',
    'Theme Changed',
    isLightMode.value
      ? 'Light mode enabled.'
      : 'Dark mode enabled.'
  )
}

/* --------------------------------
   EXPORT
-------------------------------- */

function exportBooks() {
  if (!books.value.length) {
    showToast(
      'error',
      'No Records',
      'There are no books to export yet.'
    )

    return
  }

  const data = JSON.stringify(
    books.value,
    null,
    2
  )

  const blob = new Blob(
    [data],
    {
      type: 'application/json'
    }
  )

  const url =
    URL.createObjectURL(blob)

  const link =
    document.createElement('a')

  link.href = url

  link.download =
    'atilano-library-books.json'

  document.body.appendChild(link)

  link.click()

  document.body.removeChild(link)

  URL.revokeObjectURL(url)

  showToast(
    'success',
    'Export Complete',
    'Your book records were exported successfully.'
  )
}

/* --------------------------------
   COMING SOON
-------------------------------- */

function showComingSoon(section) {
  showToast(
    'success',
    `${section}`,
    'This section is reserved for the future system expansion.'
  )

  mobileMenuOpen.value = false
}

/* --------------------------------
   TOAST
-------------------------------- */

function showToast(
  type,
  title,
  message
) {
  clearTimeout(toastTimer)

  toast.value = {
    show: true,
    type,
    title,
    message
  }

  toastTimer = setTimeout(() => {
    toast.value.show = false
  }, 4000)
}

function hideToast() {
  toast.value.show = false
}

/* --------------------------------
   HELPERS
-------------------------------- */

function getInitials(title) {
  if (!title) {
    return 'BK'
  }

  const words =
    title
      .trim()
      .split(/\s+/)
      .filter(Boolean)

  if (words.length === 1) {
    return words[0]
      .substring(0, 2)
      .toUpperCase()
  }

  return (
    words[0][0] +
    words[1][0]
  ).toUpperCase()
}

function getStatusClass(status) {
  const normalized =
    String(status ?? '')
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



