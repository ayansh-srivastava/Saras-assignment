<script setup>

defineProps({
    isDark: {
        type: Boolean,
        default: true
    },
    showEmpty: {
        type: Boolean,
        default: false
    }
})
</script>

<template>
    <div v-if="showEmpty" class="flex items-center justify-center h-60 text-center animate-fade-in">
        <p class="text-lg text-gray-500 dark:text-gray-400 px-4 font-medium tracking-wide">
            🔍 Start your search by typing something above.
        </p>
    </div>
    <slot v-else></slot>
</template>

<style scoped>
@keyframes fade-in {
    0% {
        opacity: 0;
        transform: translateY(8px);
    }

    100% {
        opacity: 1;
        transform: translateY(0);
    }
}

.animate-fade-in {
    animation: fade-in 0.6s ease-out forwards;
}
</style>
