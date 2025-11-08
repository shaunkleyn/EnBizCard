<template>
  <div v-if="show" class="profile-popup-overlay" @click.self="closePopup">
    <div class="profile-popup-content">
      <button class="profile-popup-close" @click="closePopup" aria-label="Close">
        <div class="w-6 h-6" v-html="require(`~/assets/icons/close.svg?include`)"></div>
      </button>

      <div class="profile-popup-scroll">
        <!-- Profile Preview -->
        <div v-if="profile" class="profile-popup-preview">
          <!-- Profile Image -->
          <div v-if="profile.images && profile.images.photo && profile.images.photo.url" class="profile-popup-image">
            <img :src="profile.images.photo.url" :alt="profile.genInfo.fname + ' ' + profile.genInfo.lname" />
          </div>

          <!-- Name and Title -->
          <div class="profile-popup-info">
            <h2 class="profile-popup-name">
              {{ profile.genInfo.fname }} {{ profile.genInfo.lname }}
              <span v-if="profile.genInfo.pronouns" class="profile-popup-pronouns">({{ profile.genInfo.pronouns }})</span>
            </h2>
            <p v-if="profile.genInfo.title" class="profile-popup-title">{{ profile.genInfo.title }}</p>
            <p v-if="profile.genInfo.biz" class="profile-popup-biz">{{ profile.genInfo.biz }}</p>
          </div>

          <!-- Description -->
          <p v-if="profile.genInfo.desc" class="profile-popup-desc">{{ profile.genInfo.desc }}</p>

          <!-- Primary Actions -->
          <div v-if="profile.primaryActions && profile.primaryActions.length > 0" class="profile-popup-actions">
            <div
              v-for="(action, index) in profile.primaryActions"
              :key="'action-' + index"
              class="profile-popup-action"
            >
              <a
                :href="getHref(action)"
                target="_blank"
                rel="noopener noreferrer"
                class="profile-popup-action-link"
              >
                <div class="profile-popup-action-icon">
                  <div
                    class="icon"
                    v-html="require(`~/assets/icons/${action.icon}.svg?include`)"
                  ></div>
                </div>
                <div class="profile-popup-action-text">
                  <div class="profile-popup-action-name">{{ action.name }}</div>
                  <div v-if="action.value" class="profile-popup-action-value">{{ action.value }}</div>
                </div>
              </a>
            </div>
          </div>

          <!-- Secondary Actions -->
          <div v-if="profile.secondaryActions && profile.secondaryActions.length > 0" class="profile-popup-secondary">
            <a
              v-for="(action, index) in profile.secondaryActions"
              :key="'sec-' + index"
              :href="getHref(action)"
              target="_blank"
              rel="noopener noreferrer"
              class="profile-popup-secondary-link"
              :style="{ backgroundColor: action.color }"
              :aria-label="action.name"
            >
              <div class="icon" v-html="getSVG(action)"></div>
            </a>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import utils from '@/mixins/utils'

export default {
  mixins: [utils],
  props: {
    show: {
      type: Boolean,
      default: false
    },
    profile: {
      type: Object,
      default: null
    }
  },
  methods: {
    closePopup() {
      this.$emit('close')
    },
    getHref(item) {
      if (!item.value) return '#'

      let value = item.value
      if (item.name === 'Viber') {
        value = value.replace(/\s/g, '')
      }

      if (item.href) {
        return item.href + value + (item.hrefEnd || '')
      }
      return value
    }
  }
}
</script>

<style lang="scss" scoped>
.profile-popup-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.8);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
  padding: 1rem;
}

.profile-popup-content {
  background: #1a1a1a;
  border-radius: 1rem;
  max-width: 500px;
  width: 100%;
  max-height: 90vh;
  position: relative;
  overflow: hidden;
}

.profile-popup-close {
  position: absolute;
  top: 1rem;
  right: 1rem;
  background: rgba(0, 0, 0, 0.5);
  border: none;
  border-radius: 50%;
  width: 2.5rem;
  height: 2.5rem;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  z-index: 10;
  transition: background-color 0.2s ease;

  &:hover {
    background: rgba(0, 0, 0, 0.7);
  }

  ::v-deep svg {
    fill: white;
  }
}

.profile-popup-scroll {
  overflow-y: auto;
  max-height: 90vh;
  padding: 2rem;
}

.profile-popup-preview {
  color: #eee;
}

.profile-popup-image {
  width: 120px;
  height: 120px;
  border-radius: 50%;
  overflow: hidden;
  margin: 0 auto 1.5rem;

  img {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }
}

.profile-popup-info {
  text-align: center;
  margin-bottom: 1.5rem;
}

.profile-popup-name {
  font-size: 1.5rem;
  font-weight: bold;
  margin: 0 0 0.5rem;
  color: #fff;
}

.profile-popup-pronouns {
  font-size: 1rem;
  font-weight: normal;
  color: #aaa;
}

.profile-popup-title {
  font-size: 1.1rem;
  color: #ccc;
  margin: 0.25rem 0;
}

.profile-popup-biz {
  font-size: 1rem;
  color: #aaa;
  margin: 0.25rem 0;
}

.profile-popup-desc {
  text-align: center;
  color: #ccc;
  line-height: 1.6;
  margin-bottom: 1.5rem;
  padding: 1rem;
  background: rgba(255, 255, 255, 0.05);
  border-radius: 0.5rem;
}

.profile-popup-actions {
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
  margin-bottom: 1.5rem;
}

.profile-popup-action {
  background: rgba(255, 255, 255, 0.05);
  border-radius: 0.75rem;
  transition: background-color 0.2s ease;

  &:hover {
    background: rgba(255, 255, 255, 0.1);
  }
}

.profile-popup-action-link {
  display: flex;
  align-items: center;
  gap: 1rem;
  padding: 1rem;
  text-decoration: none;
  color: inherit;
}

.profile-popup-action-icon {
  flex-shrink: 0;
  width: 3rem;
  height: 3rem;
  background: rgba(255, 255, 255, 0.1);
  border-radius: 0.5rem;
  display: flex;
  align-items: center;
  justify-content: center;

  .icon {
    width: 1.5rem;
    height: 1.5rem;

    ::v-deep svg {
      fill: #fff;
    }
  }
}

.profile-popup-action-text {
  flex: 1;
  min-width: 0;
}

.profile-popup-action-name {
  font-weight: 600;
  font-size: 0.95rem;
  color: #fff;
  margin-bottom: 0.25rem;
}

.profile-popup-action-value {
  font-size: 0.85rem;
  color: #aaa;
  word-break: break-all;
}

.profile-popup-secondary {
  display: flex;
  flex-wrap: wrap;
  gap: 0.75rem;
  justify-content: center;
}

.profile-popup-secondary-link {
  width: 3rem;
  height: 3rem;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: transform 0.2s ease;

  &:hover {
    transform: scale(1.1);
  }

  .icon {
    width: 1.5rem;
    height: 1.5rem;

    ::v-deep svg {
      fill: #fff;
    }
  }
}
</style>
