<script setup>
import { defineProps, computed, ref } from 'vue'
import SearchItem from './SearchItem.vue'
import WorkModal from './WorkModal.vue'
const { items, isDark, totalBooks } = defineProps({
    items: {
        type: Array,
        required: true
    },
    isDark: {
        type: Boolean,
        default: true
    },
    totalBooks: {
        type: Number,
        default: 0
    }
})
const selectedWork = ref(null);
const showModal = ref(false)
const authName = ref('Unknown Author')
const containerClasses = computed(() => [
    'w-full space-y-6',
])

const headerClasses = computed(() => [
    'text-xl font-bold mb-6 flex items-center gap-3 transition-colors duration-300',
    isDark ? 'text-gray-200' : 'text-gray-800'
])

const gridClasses = computed(() => [
    'grid gap-4 auto-fit-columns',
    'grid-cols-1 md:grid-cols-2 xl:grid-cols-3',
])
const handleSelect = async ({itemKey, authorName}) => {
    try {
        authName.value = authorName

        const workRes = await fetch(`https://openlibrary.org${itemKey}.json`)
        const workData = await workRes.json()

        selectedWork.value = workData
        showModal.value = true

    } catch (error) {
        console.error('Error fetching work data:', error)
    }
}
const resultCountClasses = computed(() => [
    'text-sm font-medium px-3 py-1 rounded-full transition-colors duration-300',
    isDark
        ? 'bg-blue-500/20 text-blue-300 border border-blue-500/30'
        : 'bg-blue-100 text-blue-700 border border-blue-200'
])
</script>

<template>
    <div :class="containerClasses">
        <div :class="headerClasses">
            <svg class="w-6 h-6" :class="isDark ? 'text-blue-400' : 'text-blue-500'" fill="none" stroke="currentColor"
                viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                    d="M12 6.253v13m0-13C10.832 5.477 9.246 5 7.5 5S4.168 5.477 3 6.253v13C4.168 18.477 5.754 18 7.5 18s3.332.477 4.5 1.253m0-13C13.168 5.477 14.754 5 16.5 5c1.746 0 3.332.477 4.5 1.253v13C19.832 18.477 18.246 18 16.5 18c-1.746 0-3.332.477-4.5 1.253z" />
            </svg>
            <span>Search Results</span>
            <span :class="resultCountClasses">
                {{ totalBooks }} book{{ totalBooks !== 1 ? 's' : '' }} found
            </span>
        </div>

        <div :class="gridClasses">
            <SearchItem v-for="item in items" :key="item.id" :item="item.item" :isDark="isDark" class="animate-fadeInUp"
                :itemKey="item.id" :style="{ animationDelay: `${items.indexOf(item) * 0.1}s` }"
                @select="handleSelect" />
        </div>
        <WorkModal :work="selectedWork" :visible="showModal" @close="showModal = false" :isDark="isDark" :authorName="authName || 'Unknown Author'" />
    </div>
</template>

<style scoped>
@keyframes fadeInUp {
    from {
        opacity: 0;
        transform: translateY(20px);
    }

    to {
        opacity: 1;
        transform: translateY(0);
    }
}

.animate-fadeInUp {
    animation: fadeInUp 0.6s ease-out forwards;
    opacity: 0;
}

.auto-fit-columns {
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
}

@media (min-width: 768px) {
    .auto-fit-columns {
        grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
    }
}
</style>