<template>
    <header class="fixed top-0 left-0 right-0 z-50 backdrop-blur-sm transition-all duration-700"
        :class="isDark ? 'bg-gray-900/30 border-b border-gray-700/30' : 'bg-white/30 border-b border-gray-200/30'">
        <div class="flex justify-between items-center h-20 px-6 max-w-7xl mx-auto">

            <div class="flex items-center space-x-3 logo-container">
                <img src="../assets/logo1.png" alt="Books&Books Logo" class="w-12 h-12" />
                <div class="hidden sm:block">
                    <h1 class="text-xl font-bold" :class="isDark ? 'text-gray-100' : 'text-gray-900'">Books&Books</h1>
                    <p class="text-sm opacity-70" :class="isDark ? 'text-gray-300' : 'text-gray-600'">Discover Your Next
                        Great Read</p>
                </div>
            </div>

            <button @click="$emit('toggle-theme')" class="group w-12 h-12 rounded-full ...">
                <span>{{ isDark ? '☀️' : '🌙' }}</span>
            </button>
        </div>
    </header>
</template>

<script setup>
defineProps({ isDark: Boolean })
defineEmits(['toggle-theme'])
</script>
<style scoped>
.logo-container {
    animation: float 4s ease-in-out infinite;
}
</style>