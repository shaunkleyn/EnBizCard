<template>
  <div class="team-member-editor">
    <div class="flex justify-between items-center mb-4">
      <h3 class="text-lg font-bold">Team Members</h3>
      <button
        @click="addTeamMember"
        class="px-4 py-2 bg-blue-600 hover:bg-blue-700 rounded transition-colors duration-200 flex items-center gap-2"
      >
        <div class="w-5 h-5" v-html="require(`~/assets/icons/add.svg?include`)"></div>
        Add Team Member
      </button>
    </div>

    <div v-if="teamMembers.length === 0" class="text-center py-8 text-gray-500">
      No team members added yet. Click "Add Team Member" to get started.
    </div>

    <draggable
      v-model="teamMembers"
      handle=".drag-handle"
      class="space-y-4"
    >
      <div
        v-for="(member, index) in teamMembers"
        :key="'member-' + index"
        class="team-member-card bg-gray-900 rounded-lg p-4"
      >
        <div class="flex items-start gap-3 mb-3">
          <button class="drag-handle py-1 pr-1 shrink-0 focus:outline-none cursor-move" tabindex="-1">
            <div class="w-6 h-6" v-html="require(`~/assets/icons/drag.svg?include`)"></div>
          </button>

          <div class="flex-1">
            <div class="grid grid-cols-2 gap-3 mb-3">
              <input
                v-model="member.fname"
                type="text"
                placeholder="First Name"
                class="px-3 py-2 bg-black text-white rounded border border-gray-600 focus:outline-none focus:border-gray-400"
              />
              <input
                v-model="member.lname"
                type="text"
                placeholder="Last Name"
                class="px-3 py-2 bg-black text-white rounded border border-gray-600 focus:outline-none focus:border-gray-400"
              />
            </div>

            <div class="grid grid-cols-2 gap-3 mb-3">
              <input
                v-model="member.title"
                type="text"
                placeholder="Job Title"
                class="px-3 py-2 bg-black text-white rounded border border-gray-600 focus:outline-none focus:border-gray-400"
              />
              <input
                v-model="member.pronouns"
                type="text"
                placeholder="Pronouns (optional)"
                class="px-3 py-2 bg-black text-white rounded border border-gray-600 focus:outline-none focus:border-gray-400"
              />
            </div>

            <textarea
              v-model="member.desc"
              placeholder="Description (optional)"
              rows="2"
              class="w-full px-3 py-2 bg-black text-white rounded border border-gray-600 focus:outline-none focus:border-gray-400 mb-3"
            ></textarea>

            <!-- Photo Upload -->
            <div class="mb-3">
              <label class="text-sm text-gray-400 mb-1 block">Profile Photo</label>
              <div class="flex items-center gap-3">
                <div
                  v-if="member.photo"
                  class="w-16 h-16 rounded-full overflow-hidden bg-gray-800"
                >
                  <img :src="member.photo" class="w-full h-full object-cover" />
                </div>
                <input
                  type="file"
                  accept="image/*"
                  @change="handlePhotoUpload($event, index)"
                  class="flex-1 text-sm text-gray-400 file:mr-4 file:py-2 file:px-4 file:rounded file:border-0 file:text-sm file:font-semibold file:bg-gray-700 file:text-white hover:file:bg-gray-600"
                />
                <button
                  v-if="member.photo"
                  @click="member.photo = null"
                  class="p-2 hover:bg-gray-700 rounded transition-colors"
                  title="Remove photo"
                >
                  <div class="w-5 h-5" v-html="require(`~/assets/icons/x.svg?include`)"></div>
                </button>
              </div>
            </div>

            <!-- Contact Actions -->
            <div class="mb-3">
              <label class="text-sm text-gray-400 mb-2 block">Contact Information</label>

              <div class="space-y-2">
                <div class="flex gap-2">
                  <input
                    v-model="member.email"
                    type="email"
                    placeholder="Email"
                    class="flex-1 px-3 py-2 bg-black text-white rounded border border-gray-600 focus:outline-none focus:border-gray-400"
                  />
                </div>
                <div class="flex gap-2">
                  <input
                    v-model="member.phone"
                    type="tel"
                    placeholder="Phone"
                    class="flex-1 px-3 py-2 bg-black text-white rounded border border-gray-600 focus:outline-none focus:border-gray-400"
                  />
                </div>
                <div class="flex gap-2">
                  <input
                    v-model="member.linkedin"
                    type="url"
                    placeholder="LinkedIn URL (optional)"
                    class="flex-1 px-3 py-2 bg-black text-white rounded border border-gray-600 focus:outline-none focus:border-gray-400"
                  />
                </div>
              </div>
            </div>
          </div>

          <button
            @click="removeTeamMember(index)"
            class="p-2 shrink-0 hover:bg-gray-700 rounded transition-colors"
            title="Remove team member"
          >
            <div class="w-6 h-6" v-html="require(`~/assets/icons/x.svg?include`)"></div>
          </button>
        </div>
      </div>
    </draggable>
  </div>
</template>

<script>
import draggable from 'vuedraggable'

export default {
  components: {
    draggable
  },
  props: {
    value: {
      type: Array,
      default: () => []
    }
  },
  computed: {
    teamMembers: {
      get() {
        return this.value
      },
      set(value) {
        this.$emit('input', value)
      }
    }
  },
  methods: {
    addTeamMember() {
      const newMember = {
        fname: '',
        lname: '',
        title: '',
        pronouns: '',
        desc: '',
        photo: null,
        email: '',
        phone: '',
        linkedin: ''
      }
      this.teamMembers.push(newMember)
    },
    removeTeamMember(index) {
      this.teamMembers.splice(index, 1)
    },
    handlePhotoUpload(event, index) {
      const file = event.target.files[0]
      if (!file) return

      const reader = new FileReader()
      reader.onload = (e) => {
        this.$set(this.teamMembers[index], 'photo', e.target.result)
      }
      reader.readAsDataURL(file)
    }
  }
}
</script>

<style lang="scss" scoped>
.team-member-editor {
  margin-top: 1rem;
}

.team-member-card {
  transition: background-color 0.2s ease;

  &:hover {
    background-color: rgba(31, 41, 55, 0.8);
  }
}
</style>
