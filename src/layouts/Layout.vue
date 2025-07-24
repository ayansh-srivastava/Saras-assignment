<template>
    <div class="h-full w-full flex min-h-screen flex-col text-gray-900 dark:text-gray-100 transition-all duration-700"
        :class="props.isDark ? 'bg-gradient-to-br from-gray-900 via-slate-800 to-gray-900' : 'bg-gradient-to-br from-blue-50 via-indigo-50 to-purple-50'">

        <AppHeader :isDark="props.isDark" @toggle-theme="emit('toggle-theme')" />
        <slot></slot>
        <AppFooter :isDark="props.isDark" />

    </div>
</template>

<script setup>
import { ref, watchEffect } from 'vue'
const emit = defineEmits(['toggle-theme'])
import AppHeader from '../components/AppHeader.vue'
import AppFooter from '../components/AppFooter.vue'

const props = defineProps({ isDark: Boolean })

watchEffect(() => {
    document.documentElement.classList.toggle('dark', props.isDark)
})
</script>
