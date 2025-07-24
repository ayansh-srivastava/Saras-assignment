<template>
  <div v-if="visible" class="fixed inset-0 z-50 flex items-center justify-center">
    
    <div 
      class="absolute inset-0 bg-black/75"
      @click="close"
    ></div>

    
    <div
      class="relative max-w-3xl w-full mx-4 p-6 rounded-2xl shadow-2xl bg-white dark:bg-gray-800 overflow-auto"
      :style="{ maxHeight: '90vh' }"
    >
      
      <button
        class="absolute top-4 right-4 text-gray-500 hover:text-gray-700 dark:text-gray-400 dark:hover:text-gray-200"
        @click="close"
        aria-label="Close modal"
      >
        ✕
      </button>

      
      <div class="space-y-4">
        
        <h2 class="text-2xl font-bold text-gray-900 dark:text-gray-100">
          {{ work.title }}
        </h2>

        
        <p class="text-sm text-gray-600 dark:text-gray-400">
          <span v-if="authorName">by {{ authorName }}</span>
          <span v-if="publicationYear"> ({{ publicationYear }})</span>
        </p>

        
        <img
          v-if="coverUrl"
          :src="coverUrl"
          :alt="work.title"
          class="w-full max-h-96 object-contain rounded-lg"
        />

        
        <p class="text-base text-gray-800 dark:text-gray-200 whitespace-pre-line">
          {{ work.descriptionText }}
        </p>

        
        <div v-if="subjects && subjects.length" class="flex flex-wrap gap-2">
          <span
            v-for="subj in subjects"
            :key="subj"
            class="px-3 py-1 bg-blue-100 text-blue-800 text-xs rounded-full dark:bg-blue-900 dark:text-blue-200"
          >
            {{ subj }}
          </span>
        </div>

        
        <div v-if="work.links && work.links.length" class="space-y-1">
          <h3 class="font-semibold text-gray-700 dark:text-gray-300">Links:</h3>
          <ul class="list-disc list-inside">
            <li v-for="link in work.links" :key="link.url">
              <a
                :href="link.url"
                target="_blank"
                rel="noopener"
                class="text-blue-600 dark:text-blue-400 hover:underline"
              >
                {{ link.title || link.url }}
              </a>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed, watch } from 'vue'
import { toRefs } from 'vue'

const props = defineProps({
  work: { type: Object, required: true },
  visible: { type: Boolean, default: false }
})

const emit = defineEmits(['close'])

const { work: w } = toRefs(props)


const authorName = computed(() => {
  if (w.value.authors?.length) {
    return w.value.authors.map(a => a.name || a.author?.key?.split('/')?.pop()).join(', ')
  }
  return ''
})


const publicationYear = computed(() => {
  const year = w.value.created?.value || w.value.first_publish_date
  return year?.slice(0, 4)
})


const descriptionText = computed(() => {
  if (typeof w.value.description === 'string') return w.value.description
  if (w.value.description?.value) return w.value.description.value
  return ''
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
