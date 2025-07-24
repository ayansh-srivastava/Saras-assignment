<template>
    <footer
        class="relative z-10 w-full py-6 text-center text-sm mt-auto backdrop-blur-sm transition-all duration-700 ease-out"
        :class="isDark ? 'border-t border-gray-700/30 bg-gray-900/40' : 'border-t border-white/20 bg-white/40'">
        <p class="opacity-70 hover:opacity-100 transition-opacity duration-300"
            :class="isDark ? 'text-gray-300' : 'text-gray-600'">
            © 2025 Books&Books - Discover Your Next Great Read
        </p>
    </footer>
</template>

<script setup>
defineProps({ isDark: Boolean })
</script>
