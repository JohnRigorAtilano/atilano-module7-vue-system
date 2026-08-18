<template>
  <header class="app-header">
    <!-- Mobile Menu Button -->
    <button
      class="mobile-menu-btn"
      type="button"
      @click="$emit('toggle-menu')"
      aria-label="Toggle navigation menu"
    >
      <span></span>
      <span></span>
      <span></span>
    </button>

    <!-- Page Heading -->
    <div class="header-title">
      <div>
        <p class="eyebrow">LIBRARY MANAGEMENT SYSTEM</p>
        <h1>{{ title }}</h1>
        <p class="subtitle">{{ subtitle }}</p>
      </div>
    </div>

    <!-- Header Actions -->
    <div class="header-actions">

      <!-- Search -->
      <div class="global-search">
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
          type="text"
          placeholder="Search books..."
          :value="searchTerm"
          @input="$emit('update:searchTerm', $event.target.value)"
        />

        <span class="shortcut">Ctrl K</span>
      </div>

      <!-- Notification -->
      <button class="icon-btn" type="button" title="Notifications">
        <svg
          viewBox="0 0 24 24"
          fill="none"
          stroke="currentColor"
          stroke-width="1.8"
        >
          <path
            d="M18 8a6 6 0 0 0-12 0c0 7-3 7-3 9h18c0-2-3-2-3-9"
          ></path>
          <path d="M13.73 21a2 2 0 0 1-3.46 0"></path>
        </svg>
        <span class="notification-dot"></span>
      </button>

      <!-- Theme Button -->
      <button
        class="icon-btn"
        type="button"
        title="Theme"
        @click="$emit('toggle-theme')"
      >
        <svg
          viewBox="0 0 24 24"
          fill="none"
          stroke="currentColor"
          stroke-width="1.8"
        >
          <path d="M21 12.79A9 9 0 1 1 11.21 3 7 7 0 0 0 21 12.79Z"></path>
        </svg>
      </button>

      <!-- User -->
      <div class="user-profile">
        <div class="avatar">
          JR
        </div>

        <div class="user-info">
          <strong>John Rigor Atilano</strong>
          <span>Librarian</span>
        </div>

        <svg
          class="chevron"
          viewBox="0 0 24 24"
          fill="none"
          stroke="currentColor"
          stroke-width="2"
        >
          <path d="m6 9 6 6 6-6"></path>
        </svg>
      </div>
    </div>
  </header>
</template>

<script setup>
defineProps({
  title: {
    type: String,
    default: 'Dashboard'
  },

  subtitle: {
    type: String,
    default: 'Manage your library efficiently'
  },

  searchTerm: {
    type: String,
    default: ''
  }
})

defineEmits([
  'toggle-menu',
  'toggle-theme',
  'update:searchTerm'
])
</script>