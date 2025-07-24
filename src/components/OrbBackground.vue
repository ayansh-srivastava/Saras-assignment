<template>
    <div class="fixed inset-0 overflow-hidden pointer-events-none z-40">
        <div v-for="(orb, idx) in orbs" :key="idx" :class="[
            `absolute ${orb.position} w-${orb.size} h-${orb.size} rounded-full background-orb`,
            isDark ? orb.darkBg : orb.lightBg
        ]" :style="{ 'animation-delay': `${orb.delay}s` }" />
    </div>
</template>

<script setup>
import { inject } from 'vue'
const isDark = inject('isDark')

const orbs = [
    { position: 'top-20 left-20', size: '4', delay: 0, darkBg: 'bg-purple-400/30', lightBg: 'bg-blue-400/20' },
    { position: 'top-1/3 right-1/4', size: '3', delay: 2, darkBg: 'bg-cyan-400/40', lightBg: 'bg-indigo-400/30' },
    { position: 'bottom-1/3 left-1/5', size: '5', delay: 4, darkBg: 'bg-blue-400/25', lightBg: 'bg-purple-400/15' },
    { position: 'top-1/2 left-1/2', size: '2', delay: 6, darkBg: 'bg-purple-400/50', lightBg: 'bg-cyan-400/40' },
    { position: 'top-3/4 right-1/3', size: '3', delay: 8, darkBg: 'bg-pink-400/30', lightBg: 'bg-rose-400/20' }
]
</script>

<style scoped>
@keyframes float {

    0%,
    100% {
        transform: translateY(0)
    }

    50% {
        transform: translateY(-10px)
    }
}

@keyframes glow {

    0%,
    100% {
        opacity: 0.5
    }

    50% {
        opacity: 1
    }
}

.background-orb {
    animation: float 8s ease-in-out infinite, glow 4s ease-in-out infinite;
}
</style>