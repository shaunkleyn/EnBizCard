<template>
  <div class="stepC mt-6">
    <div class="flex">
      <button class="py-1 pr-1 shrink-0 focus:outline-none drag cursor-move" tabindex="-1">
        <div class="w-6 h-6" v-html="require(`~/assets/icons/drag.svg?include`)"></div>
      </button>
      <div
        class="p-3 shrink-0 rounded-l"
        :style="{
          background: `${name == 'secondaryActions' ? item.color : buttonBg
            }`,
        }"
        :title="item.name"
      >
        <div
          class="w-6 h-6"
          :class="name == 'secondaryActions' ? null : 'action'"
          v-html="getSVG(item)"
        ></div>
      </div>
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
    <div v-if="name === 'primaryActions'" class="ml-12 mt-2 p-3 bg-gray-900 rounded">
      <div class="mb-2">
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
  </div>
</template>

<script>
import utils from '@/mixins/utils'
export default {
  props: ['name', 'item', 'index', 'type', 'buttonBg', 'removeAction'],
  mixins: [utils],
  mounted() {
    this.$refs.input.focus()
  },
}
</script>
