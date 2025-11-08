<template>
  <div class="collapsible-section">
    <div
      class="collapsible-header"
      :class="headerClass"
      role="button"
      tabindex="0"
      :aria-expanded="isExpanded.toString()"
    >
      <button
        v-if="draggable"
        type="button"
        class="drag-handle section-drag"
        aria-label="Drag to reorder section"
        @click.stop
      >
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="20"
          height="20"
          viewBox="0 0 24 24"
          fill="none"
          stroke="currentColor"
          stroke-width="2"
          stroke-linecap="round"
          stroke-linejoin="round"
        >
          <line x1="3" y1="12" x2="21" y2="12"></line>
          <line x1="3" y1="6" x2="21" y2="6"></line>
          <line x1="3" y1="18" x2="21" y2="18"></line>
        </svg>
      </button>
      <div
        class="title-wrapper"
        @click="toggle"
        @keypress.enter.space.prevent="toggle"
      >
        <component :is="headingTag" :class="titleClass">
          {{ title }}
        </component>
      </div>
      <div
        class="toggle-icon"
        :class="{ 'rotate-180': !isExpanded }"
        @click="toggle"
      >
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="24"
          height="24"
          viewBox="0 0 24 24"
          fill="none"
          stroke="currentColor"
          stroke-width="2"
          stroke-linecap="round"
          stroke-linejoin="round"
        >
          <polyline points="18 15 12 9 6 15"></polyline>
        </svg>
      </div>
    </div>
    <transition name="collapse">
      <div
        v-show="isExpanded"
        class="collapsible-content"
        :class="contentClass"
      >
        <slot></slot>
      </div>
    </transition>
  </div>
</template>

<script>
export default {
  name: 'CollapsibleSection',
  props: {
    title: {
      type: String,
      required: true
    },
    defaultExpanded: {
      type: Boolean,
      default: true
    },
    draggable: {
      type: Boolean,
      default: false
    },
    headingTag: {
      type: String,
      default: 'h2',
      validator: (value) => ['h1', 'h2', 'h3', 'h4', 'h5', 'h6'].includes(value)
    },
    headerClass: {
      type: String,
      default: ''
    },
    titleClass: {
      type: String,
      default: ''
    },
    contentClass: {
      type: String,
      default: ''
    },
    storageKey: {
      type: String,
      default: null
    }
  },
  data() {
    return {
      isExpanded: this.defaultExpanded
    }
  },
  mounted() {
    // Restore state from localStorage if storageKey is provided
    if (this.storageKey && typeof window !== 'undefined') {
      const saved = localStorage.getItem(this.storageKey)
      if (saved !== null) {
        this.isExpanded = saved === 'true'
      }
    }
  },
  methods: {
    toggle() {
      this.isExpanded = !this.isExpanded

      // Save state to localStorage if storageKey is provided
      if (this.storageKey && typeof window !== 'undefined') {
        localStorage.setItem(this.storageKey, this.isExpanded.toString())
      }

      this.$emit('toggle', this.isExpanded)
    }
  }
}
</script>

<style scoped>
.collapsible-section {
  margin-bottom: 1rem;
}

.collapsible-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  user-select: none;
  transition: all 0.2s ease;
  padding: 0.5rem 0;
  gap: 0.75rem;
}

.drag-handle {
  flex-shrink: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 2rem;
  height: 2rem;
  border: none;
  background: transparent;
  color: #9ca3af;
  cursor: grab;
  border-radius: 0.25rem;
  transition: all 0.2s ease;
  padding: 0.25rem;
}

.drag-handle:hover {
  background-color: rgba(75, 85, 99, 0.5);
  color: #d1d5db;
}

.drag-handle:active {
  cursor: grabbing;
}

.title-wrapper {
  flex: 1;
  cursor: pointer;
  padding: 0.25rem 0;
}

.title-wrapper:hover {
  opacity: 0.8;
}

.collapsible-header:focus {
  outline: 2px solid rgba(59, 130, 246, 0.5);
  outline-offset: 2px;
  border-radius: 0.25rem;
}

.toggle-icon {
  flex-shrink: 0;
  transition: transform 0.3s ease;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 2rem;
  height: 2rem;
}

.toggle-icon.rotate-180 {
  transform: rotate(180deg);
}

.collapsible-content {
  overflow: hidden;
}

/* Collapse transition */
.collapse-enter-active,
.collapse-leave-active {
  transition: all 0.3s ease;
  max-height: 5000px;
}

.collapse-enter,
.collapse-leave-to {
  opacity: 0;
  max-height: 0;
  margin-top: 0;
  margin-bottom: 0;
}
</style>
