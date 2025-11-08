<template>
  <div v-if="show" class="icon-picker-overlay" @click.self="close">
    <div class="icon-picker-modal">
      <div class="icon-picker-header">
        <h3>Select an Icon</h3>
        <button @click="close" class="close-btn">
          <span v-html="require(`~/assets/icons/close.svg?include`)"></span>
        </button>
      </div>

      <div class="search-container">
        <input
          v-model="searchQuery"
          type="text"
          placeholder="Search icons..."
          class="search-input"
          @input="filterIcons"
        />
      </div>

      <div class="icons-grid">
        <button
          v-for="icon in filteredIcons"
          :key="icon"
          class="icon-item"
          :class="{ selected: icon === selectedIcon }"
          @click="selectIcon(icon)"
          :title="icon"
        >
          <span v-html="require(`~/assets/icons/${icon}.svg?include`)"></span>
          <span class="icon-name">{{ formatIconName(icon) }}</span>
        </button>
      </div>

      <div v-if="filteredIcons.length === 0" class="no-results">
        No icons found for "{{ searchQuery }}"
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'IconPicker',
  props: {
    show: {
      type: Boolean,
      default: false,
    },
    currentIcon: {
      type: String,
      default: '',
    },
  },
  data() {
    return {
      searchQuery: '',
      selectedIcon: this.currentIcon,
      allIcons: [
        'add-img',
        'add-user',
        'add',
        'appstore',
        'arrow-right',
        'artstation',
        'behance',
        'bitcoin',
        'brand',
        'buymeacoffee',
        'calendar',
        'call',
        'cashapp',
        'check',
        'close',
        'code',
        'codeberg',
        'copy',
        'diaspora',
        'discord',
        'download',
        'drag',
        'dribbble',
        'ellipsis',
        'email',
        'facebook',
        'file',
        'friendica',
        'funkwhale',
        'github',
        'gitlab',
        'imo',
        'instagram',
        'irc',
        'key',
        'line',
        'linkedin',
        'location',
        'logo',
        'mastodon',
        'matrix',
        'medium',
        'messenger',
        'monero',
        'open-collective',
        'patreon',
        'pause',
        'paypal',
        'peertube',
        'pinterest',
        'pixelfed',
        'play',
        'playstore',
        'product',
        'qrcode',
        'quora',
        'reddit',
        'share',
        'signal',
        'siilo',
        'skype',
        'sms',
        'snapchat',
        'soundcloud',
        'spotify',
        'store',
        'telegram',
        'text',
        'threads',
        'tiktok',
        'tumblr',
        'twitch',
        'twitter',
        'viber',
        'vimeo',
        'vk',
        'website',
        'wechat',
        'whatsapp',
        'x',
        'xmpp',
        'yelp',
        'youtube',
      ],
      filteredIcons: [],
    }
  },
  watch: {
    show(newVal) {
      if (newVal) {
        this.selectedIcon = this.currentIcon
        this.searchQuery = ''
        this.filteredIcons = [...this.allIcons]
      }
    },
    currentIcon(newVal) {
      this.selectedIcon = newVal
    },
  },
  mounted() {
    this.filteredIcons = [...this.allIcons]
  },
  methods: {
    close() {
      this.$emit('close')
    },
    selectIcon(icon) {
      this.selectedIcon = icon
      this.$emit('select', icon)
      this.close()
    },
    filterIcons() {
      const query = this.searchQuery.toLowerCase().trim()
      if (!query) {
        this.filteredIcons = [...this.allIcons]
      } else {
        this.filteredIcons = this.allIcons.filter((icon) =>
          icon.toLowerCase().includes(query)
        )
      }
    },
    formatIconName(icon) {
      return icon
        .split('-')
        .map((word) => word.charAt(0).toUpperCase() + word.slice(1))
        .join(' ')
    },
  },
}
</script>

<style scoped lang="scss">
.icon-picker-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.75);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 9999;
  padding: 1rem;
}

.icon-picker-modal {
  background: #1a1a1a;
  border-radius: 1rem;
  width: 100%;
  max-width: 600px;
  max-height: 80vh;
  display: flex;
  flex-direction: column;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.5);
  border: 1px solid #333;
}

.icon-picker-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1.5rem;
  border-bottom: 1px solid #333;

  h3 {
    margin: 0;
    font-size: 1.25rem;
    font-weight: 600;
    color: white;
  }

  .close-btn {
    background: transparent;
    border: none;
    cursor: pointer;
    padding: 0.5rem;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 0.5rem;
    transition: background-color 0.2s ease;

    &:hover {
      background-color: rgba(255, 255, 255, 0.1);
    }

    ::v-deep svg {
      width: 1.5rem;
      height: 1.5rem;
      fill: white;
    }
  }
}

.search-container {
  padding: 1rem 1.5rem;
  border-bottom: 1px solid #333;
}

.search-input {
  width: 100%;
  padding: 0.75rem 1rem;
  background: #0a0a0a;
  border: 1px solid #444;
  border-radius: 0.5rem;
  color: white;
  font-size: 0.95rem;
  outline: none;
  transition: border-color 0.2s ease;

  &:focus {
    border-color: #666;
  }

  &::placeholder {
    color: #888;
  }
}

.icons-grid {
  padding: 1.5rem;
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(80px, 1fr));
  gap: 0.75rem;
  overflow-y: auto;
  max-height: 400px;

  &::-webkit-scrollbar {
    width: 8px;
  }

  &::-webkit-scrollbar-track {
    background: #0a0a0a;
  }

  &::-webkit-scrollbar-thumb {
    background: #444;
    border-radius: 4px;

    &:hover {
      background: #555;
    }
  }
}

.icon-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 0.75rem;
  background: #0a0a0a;
  border: 2px solid #333;
  border-radius: 0.75rem;
  cursor: pointer;
  transition: all 0.2s ease;
  gap: 0.5rem;

  ::v-deep svg {
    width: 2rem;
    height: 2rem;
    fill: white;
  }

  .icon-name {
    font-size: 0.65rem;
    color: #999;
    text-align: center;
    line-height: 1.2;
    max-width: 100%;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }

  &:hover {
    border-color: #555;
    background: #1a1a1a;
    transform: translateY(-2px);
  }

  &.selected {
    border-color: #4ade80;
    background: rgba(74, 222, 128, 0.1);

    .icon-name {
      color: #4ade80;
    }
  }
}

.no-results {
  padding: 3rem 1.5rem;
  text-align: center;
  color: #888;
  font-size: 0.95rem;
}
</style>
