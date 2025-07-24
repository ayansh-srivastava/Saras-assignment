<script setup>
import { ref, computed } from 'vue'
import { debounce } from 'lodash-es'

const emit = defineEmits(['search'])
const props = defineProps({
  isDark: {
    type: Boolean,
    default: true
  }
})

const searchQuery = ref('')

const containerClasses = computed(() => [
  'relative flex w-full rounded-xl overflow-hidden transition-all duration-300',
  'max-w-sm sm:max-w-md md:max-w-lg lg:max-w-xl xl:max-w-2xl',
  props.isDark
    ? 'bg-gray-800 border border-gray-700 shadow-2xl shadow-black/20'
    : 'bg-white border border-gray-200 shadow-xl shadow-gray-500/10'
])

const inputClasses = computed(() => [
  'w-full pl-12 pr-4 py-3 focus:outline-none transition-colors duration-300',
  props.isDark
    ? 'text-gray-100 placeholder-gray-400 bg-gray-800'
    : 'text-gray-900 placeholder-gray-500 bg-white'
])

const iconStroke = computed(() => (props.isDark ? '#e5e7eb' : '#374151'))

const debounceSearch = debounce(() => {
  emit('search', searchQuery.value)
  console.log('Searching for:', searchQuery.value)
}, 400)
</script>

<template>
  <div :class="containerClasses">
    <div class="absolute left-4 top-1/2 transform -translate-y-1/2">
      <svg
        xmlns="http://www.w3.org/2000/svg"
        class="h-5 w-5 transition-colors duration-300"
        fill="none"
        viewBox="0 0 24 24"
        :stroke="iconStroke"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          stroke-width="2"
          d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"
        />
      </svg>
    </div>
    <input
      v-model="searchQuery"
      :class="inputClasses"
      placeholder="Search books..."
      @input="debounceSearch"
    />
  </div>
</template>
