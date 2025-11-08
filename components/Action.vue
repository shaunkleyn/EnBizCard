<template>
  <div class="stepC mt-6">
    <div class="flex">
      <button class="py-1 pr-1 shrink-0 focus:outline-none drag cursor-move" tabindex="-1">
        <div class="w-6 h-6" v-html="require(`~/assets/icons/drag.svg?include`)"></div>
      </button>
      <button
        class="p-3 shrink-0 rounded-l cursor-pointer hover:opacity-80 transition-opacity duration-200"
        :style="{
          background: `${name == 'secondaryActions' ? item.color : (item.iconBgColor || buttonBg)}`,
        }"
        :title="'Click to change icon'"
        @click="openIconPicker"
        tabindex="-1"
      >
        <div
          class="w-6 h-6"
          :class="name == 'secondaryActions' ? null : 'action'"
          :style="{ fill: item.iconColor || 'inherit' }"
          v-html="getSVG(item)"
        ></div>
      </button>
      <!-- // TODO show title content when input is focused. -->
      <div class="w-full">
        <input
          ref="input"
          class="px-4 w-full h-12 bg-black placeholder-gray-600 rounded-r border border-transparent transition-colors duration-200 focus:outline-none focus:border-gray-600 hover:border-gray-600"
          type="text"
          :aria-label="'Enter ' + item.label"
          :title="'Enter ' + item.label"
          v-model="type[index].value"
          :placeholder="item.placeholder"
        />
      </div>
      <button
        class="p-1 m-2 shrink-0 focus:outline-none rounded hover:bg-gray-700 focus:bg-gray-700 transition-colors duration-200"
        @click="removeAction(name, index)"
        :aria-label="'Remove ' + item.label"
        title="Remove field"
      >
        <div class="w-6 h-6" v-html="require(`~/assets/icons/x.svg?include`)"></div>
      </button>
    </div>

    <!-- Display format options for primary actions -->
    <div v-if="name === 'primaryActions'" class="ml-12 mt-2 p-3 bg-gray-900 rounded space-y-3">
      <!-- Icon Customization -->
      <div class="pb-3 border-b border-gray-700">
        <h4 class="text-sm font-medium text-gray-300 mb-2">Icon Customization</h4>

        <div class="grid grid-cols-2 gap-2 mb-2">
          <div>
            <label class="text-xs text-gray-400 mb-1 block">Icon Color:</label>
            <div class="flex items-center gap-2">
              <input
                v-model="type[index].iconColor"
                type="color"
                class="w-10 h-8 rounded border border-gray-600 cursor-pointer bg-black"
              />
              <input
                v-model="type[index].iconColor"
                type="text"
                placeholder="#FFFFFF"
                class="flex-1 px-2 py-1 text-xs bg-black text-white rounded border border-gray-600 focus:outline-none focus:border-gray-400"
              />
            </div>
          </div>

          <div>
            <label class="text-xs text-gray-400 mb-1 block">Background Color:</label>
            <div class="flex items-center gap-2">
              <input
                v-model="type[index].iconBgColor"
                type="color"
                class="w-10 h-8 rounded border border-gray-600 cursor-pointer bg-black"
              />
              <input
                v-model="type[index].iconBgColor"
                type="text"
                placeholder="#000000"
                class="flex-1 px-2 py-1 text-xs bg-black text-white rounded border border-gray-600 focus:outline-none focus:border-gray-400"
              />
            </div>
          </div>
        </div>

        <div>
          <label class="text-xs text-gray-400 mb-1 flex items-center">
            <input
              type="checkbox"
              v-model="type[index].showTitle"
              class="mr-2"
            />
            Show title label ({{ item.name }})
          </label>
        </div>
      </div>

      <!-- Display Format -->
      <div>
        <label class="text-sm text-gray-400 mb-1 block">Display Format:</label>
        <select
          v-model="type[index].displayFormat"
          class="px-3 py-2 bg-black text-white rounded border border-gray-600 focus:outline-none focus:border-gray-400"
        >
          <option value="icon-only">Icon Only</option>
          <option value="row">Row (Icon + Text + Button)</option>
        </select>
      </div>

      <div v-if="type[index].displayFormat === 'row'" class="space-y-2">
        <div>
          <label class="text-sm text-gray-400 mb-1 flex items-center">
            <input
              type="checkbox"
              v-model="type[index].showValue"
              class="mr-2"
            />
            Show value (email, phone, etc.)
          </label>
        </div>

        <div>
          <label class="text-sm text-gray-400 mb-1 block">Button Text (optional):</label>
          <input
            v-model="type[index].buttonText"
            type="text"
            placeholder="e.g., 'Call Me', 'Email Me', 'Visit'"
            class="px-3 py-2 w-full bg-black text-white rounded border border-gray-600 focus:outline-none focus:border-gray-400"
          />
        </div>
      </div>
    </div>

    <!-- Icon Picker Modal -->
    <IconPicker
      :show="showIconPicker"
      :current-icon="item.icon"
      @select="onIconSelect"
      @close="showIconPicker = false"
    />
  </div>
</template>

<script>
import utils from '@/mixins/utils'
import IconPicker from './IconPicker.vue'

export default {
  components: {
    IconPicker,
  },
  props: ['name', 'item', 'index', 'type', 'buttonBg', 'removeAction'],
  mixins: [utils],
  data() {
    return {
      showIconPicker: false,
    }
  },
  mounted() {
    this.$refs.input.focus()
    // Initialize default values if not set
    if (this.name === 'primaryActions') {
      if (!this.type[this.index].iconColor) {
        this.$set(this.type[this.index], 'iconColor', '#ffffff')
      }
      if (!this.type[this.index].iconBgColor) {
        this.$set(this.type[this.index], 'iconBgColor', this.buttonBg || '#000000')
      }
      if (this.type[this.index].showTitle === undefined) {
        this.$set(this.type[this.index], 'showTitle', true)
      }
    }
  },
  methods: {
    openIconPicker() {
      if (this.name === 'primaryActions') {
        this.showIconPicker = true
      }
    },
    onIconSelect(icon) {
      this.$set(this.type[this.index], 'icon', icon)
    },
  },
}
</script>
