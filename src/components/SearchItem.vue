<script setup>
import { defineProps, computed } from 'vue'

const { item, isDark, itemKey } = defineProps({
    item: {
        type: Object,
        required: true
    },
    isDark: {
        type: Boolean,
        default: true
    },
    itemKey: {
        type: String,
        required: true
    }
})
const emit = defineEmits(['select'])
const coverImageUrl = computed(() => {
    if (item.coverId) {
        return `https://covers.openlibrary.org/b/id/${item.coverId}-M.jpg`
    }
    return null
})

const cardClasses = computed(() => [
    'group relative overflow-hidden rounded-2xl transition-all duration-300 hover:scale-[1.02] hover:-translate-y-1',
    'border backdrop-blur-sm z-50',
    isDark
        ? 'bg-gray-800/60 border-gray-700/50 hover:bg-gray-800/80 hover:border-gray-600/70 shadow-lg shadow-black/20 hover:shadow-2xl hover:shadow-black/30'
        : 'bg-white/70 border-gray-200/60 hover:bg-white/90 hover:border-gray-300/80 shadow-lg shadow-gray-500/10 hover:shadow-xl hover:shadow-gray-500/20'
])

const titleClasses = computed(() => [
    'font-bold text-lg mb-2 line-clamp-2 group-hover:text-transparent group-hover:bg-clip-text transition-all duration-300',
    isDark
        ? 'text-gray-100 group-hover:bg-gradient-to-r group-hover:from-blue-400 group-hover:to-purple-400'
        : 'text-gray-900 group-hover:bg-gradient-to-r group-hover:from-blue-600 group-hover:to-purple-600'
])

const authorClasses = computed(() => [
    'text-sm mb-1 font-medium transition-colors duration-300',
    isDark ? 'text-gray-300 group-hover:text-gray-200' : 'text-gray-600 group-hover:text-gray-700'
])

const yearClasses = computed(() => [
    'text-xs font-medium px-2 py-1 rounded-full inline-block transition-all duration-300',
    isDark
        ? 'text-gray-400 bg-gray-700/50 group-hover:text-gray-300 group-hover:bg-gray-600/60'
        : 'text-gray-500 bg-gray-100/80 group-hover:text-gray-600 group-hover:bg-gray-200/80'
])
const authorName=item.author?.join(', ') || 'Unknown Author'
</script>

<template>
    <div :class="cardClasses" @click="$emit('select', {itemKey, authorName})">
        <div class="absolute inset-0 opacity-0 group-hover:opacity-10 transition-opacity duration-300"
            :class="isDark ? 'bg-gradient-to-br from-blue-500 to-purple-500' : 'bg-gradient-to-br from-blue-400 to-purple-400'">
        </div>

        <div class="relative z-50 p-6 flex gap-4">
            <div class="flex-shrink-0">
                <div class="w-16 h-20 rounded-lg overflow-hidden shadow-md transition-transform duration-300 group-hover:scale-105"
                    :class="isDark ? 'bg-gray-700/50' : 'bg-gray-200/50'">
                    <img v-if="coverImageUrl" :src="coverImageUrl" :alt="`Cover of ${item.title}`"
                        class="w-full h-full object-cover" @error="$event.target.style.display = 'none'" />
                    <div v-else class="w-full h-full flex items-center justify-center">
                        <svg class="w-8 h-8 opacity-50" :class="isDark ? 'text-gray-500' : 'text-gray-400'"
                            fill="currentColor" viewBox="0 0 20 20">
                            <path d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z" />
                        </svg>
                    </div>
                </div>
            </div>

            <div class="flex-1 min-w-0">
                <h3 :class="titleClasses">
                    {{ item.title }}
                </h3>

                <div class="space-y-2">
                    <p :class="authorClasses">
                        <span class="opacity-75">by </span>
                        <span class="font-semibold">{{ item.author?.join(', ') || 'Unknown Author' }}</span>
                    </p>

                    <div class="flex items-center gap-2">
                        <span v-if="item.year" :class="yearClasses">
                            📅 {{ item.year }}
                        </span>

                        <div
                            class="flex items-center gap-1 opacity-60 group-hover:opacity-80 transition-opacity duration-300">
                            <svg class="w-4 h-4" :class="isDark ? 'text-blue-400' : 'text-blue-500'" fill="currentColor"
                                viewBox="0 0 20 20">
                                <path d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z" />
                            </svg>
                            <span class="text-xs font-medium" :class="isDark ? 'text-gray-400' : 'text-gray-500'">
                                Available
                            </span>
                        </div>
                    </div>
                </div>
            </div>

            <div
                class="flex-shrink-0 flex items-center opacity-0 group-hover:opacity-100 transition-all duration-300 transform translate-x-2 group-hover:translate-x-0">
                <svg class="w-5 h-5" :class="isDark ? 'text-blue-400' : 'text-blue-500'" fill="none"
                    stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7" />
                </svg>
            </div>
        </div>
    </div>
</template>

<style scoped>
.line-clamp-2 {
    display: -webkit-box;
    -webkit-line-clamp: 2;
    line-clamp: 2;
    -webkit-box-orient: vertical;
    overflow: hidden;
}
</style>