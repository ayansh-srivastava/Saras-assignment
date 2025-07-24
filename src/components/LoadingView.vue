<script setup>
import { computed } from 'vue'

const props = defineProps({
    isDark: {
        type: Boolean,
        default: true
    },
    isLoading: {
        type: Boolean,
        default: false
    }
})

const containerClasses = computed(() => [
    'flex flex-col items-center justify-center py-16 space-y-4 transition-all duration-500',
    props.isLoading ? 'opacity-100 scale-100' : 'opacity-0 scale-95 pointer-events-none'
])

const spinnerClasses = computed(() => [
    'w-12 h-12 border-4 rounded-full animate-spin transition-colors duration-300',
    props.isDark
        ? 'border-gray-600 border-t-blue-400'
        : 'border-gray-300 border-t-blue-500'
])

const textClasses = computed(() => [
    'text-sm font-medium animate-pulse transition-colors duration-300',
    props.isDark ? 'text-gray-300' : 'text-gray-600'
])
</script>

<template>
    <div v-if="isLoading" :class="containerClasses">
        <div class="relative">
            <div :class="spinnerClasses"></div>
            <div class="absolute inset-2 rounded-full animate-ping opacity-20"
                :class="props.isDark ? 'bg-blue-400' : 'bg-blue-500'"></div>
        </div>

        <p :class="textClasses">
            Searching through thousands of books...
        </p>

        <div class="flex space-x-2">
            <div v-for="i in 3" :key="i" class="w-2 h-2 rounded-full animate-bounce"
                :class="props.isDark ? 'bg-blue-400' : 'bg-blue-500'" :style="{ animationDelay: `${(i - 1) * 0.2}s` }">
            </div>
        </div>
    </div>
</template>

<style scoped>
@keyframes spin {
    to {
        transform: rotate(360deg);
    }
}

@keyframes bounce {

    0%,
    80%,
    100% {
        transform: scale(0);
    }

    40% {
        transform: scale(1);
    }
}

.animate-spin {
    animation: spin 1s linear infinite;
}

.animate-bounce {
    animation: bounce 1.4s infinite ease-in-out both;
}
</style>