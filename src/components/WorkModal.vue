<template>
  <div v-if="visible" class="fixed inset-0 z-50 overflow-y-auto">
    <div
      class="fixed inset-0 z-0"
      :class="isDark ? 'bg-black/80' : 'bg-black/60'"
      @click="close"
    ></div>

    <div
      class="min-h-screen flex flex-col items-center p-4 pt-10 relative z-10"
      @click.self="close"
    >
      <div
        :class="[
          'relative max-w-3xl w-full p-6 rounded-xl shadow-xl border bg-opacity-100',
          isDark 
            ? 'bg-gray-900 border-gray-700' 
            : 'bg-white border-gray-200'
        ]"
        @click.stop
      >
        <button
          :class="[
            'absolute top-4 right-4 w-8 h-8 rounded-full flex items-center justify-center',
            isDark 
              ? 'text-gray-400 hover:text-gray-200 hover:bg-gray-700' 
              : 'text-gray-500 hover:text-gray-700 hover:bg-gray-100'
          ]"
          @click="close"
          aria-label="Close modal"
        >
          <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
          </svg>
        </button>

        <div class="space-y-4">
          <h2 :class="[
            'text-2xl font-bold transition-colors duration-300',
            isDark ? 'text-gray-100' : 'text-gray-900'
          ]">
            {{ work.title }}
          </h2>

          <p :class="[
            'text-sm font-medium transition-colors duration-300',
            isDark ? 'text-gray-400' : 'text-gray-600'
          ]">
            <span v-if="authorName">by {{ props.authorName }}</span>
            <span v-if="publicationYear"> ({{ publicationYear }})</span>
          </p>

          <div class="w-full h-96 overflow-hidden rounded-lg mb-4 flex items-center justify-center bg-gray-800/20">
            <img
              v-if="coverUrl"
              :src="coverUrl"
              :alt="work.title"
              loading="lazy"
              :class="[
                'max-h-full max-w-full object-contain transition-all duration-300 hover:scale-105',
                isDark ? 'shadow-lg shadow-black/50' : 'shadow-lg shadow-gray-500/20'
              ]"
            />
          </div>

          <p :class="[
            'text-base whitespace-pre-line leading-relaxed mb-6 transition-colors duration-300',
            isDark ? 'text-gray-200' : 'text-gray-800'
          ]">
            {{ descriptionText }}
          </p>

          <div v-if="subjects && subjects.length" class="flex flex-wrap gap-2 mb-6">
            <span
              v-for="subj in subjects"
              :key="subj"
              :class="[
                'px-3 py-1 text-xs rounded-full font-medium transition-all duration-300 hover:scale-105',
                isDark 
                  ? 'bg-blue-900/50 text-blue-300 border border-blue-800/50' 
                  : 'bg-blue-100 text-blue-800 border border-blue-200'
              ]"
            >
              {{ subj }}
            </span>
          </div>

          <div v-if="work.links && work.links.length" class="space-y-2">
            <h3 :class="[
              'font-semibold text-lg mb-3 transition-colors duration-300',
              isDark ? 'text-gray-300' : 'text-gray-700'
            ]">
              External Links:
            </h3>
            <ul class="space-y-2">
              <li v-for="link in work.links" :key="link.url" class="flex items-center">
                <svg class="w-4 h-4 mr-2 flex-shrink-0" :class="isDark ? 'text-blue-400' : 'text-blue-600'" fill="currentColor" viewBox="0 0 20 20">
                  <path fill-rule="evenodd" d="M12.586 4.586a2 2 0 112.828 2.828l-3 3a2 2 0 01-2.828 0 1 1 0 00-1.414 1.414 4 4 0 005.656 0l3-3a4 4 0 00-5.656-5.656l-1.5 1.5a1 1 0 101.414 1.414l1.5-1.5zm-5 5a2 2 0 012.828 0 1 1 0 101.414-1.414 4 4 0 00-5.656 0l-3 3a4 4 0 105.656 5.656l1.5-1.5a1 1 0 10-1.414-1.414l-1.5 1.5a2 2 0 11-2.828-2.828l3-3z" clip-rule="evenodd" />
                </svg>
                <a
                  :href="link.url"
                  target="_blank"
                  rel="noopener"
                  :class="[
                    'hover:underline transition-colors duration-300 font-medium',
                    isDark ? 'text-blue-400 hover:text-blue-300' : 'text-blue-600 hover:text-blue-700'
                  ]"
                >
                  {{ link.title || link.url }}
                </a>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed, toRefs } from 'vue'

const props = defineProps({
  work: { type: Object, required: true },
  visible: { type: Boolean, default: false },
  isDark: { type: Boolean, default: true },
  authorName: { type: String, default: 'Unknown Author' }
})

const emit = defineEmits(['close'])
const { work: w } = toRefs(props)

// authorName.value = props.authorName

const publicationYear = computed(() => {
  const year = w.value.created?.value || w.value.first_publish_date
  return year?.slice(0, 4)
})

const descriptionText = computed(() => {
  if (typeof w.value.description === 'string') return w.value.description
  if (w.value.description?.value) return w.value.description.value
  return 'No description available.'
})

const subjects = computed(() =>
  (w.value.subjects || w.value.subject_people || []).slice(0, 10)
)

const coverUrl = computed(() => {
  const id = w.value.covers?.find(c => c > 0)
  return id ? `https://covers.openlibrary.org/b/id/${id}-L.jpg` : ''
})

const close = () => emit('close')
</script>
