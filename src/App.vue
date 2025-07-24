<script setup>
import { ref, watchEffect } from 'vue'
import SearchBar from './components/SearchBar.vue'
import LoadingView from './components/LoadingView.vue'
import EmptySearch from './components/EmptySearch.vue'
import dummyData from './assets/dummyData.js'

const isDark = ref(true)
const isLoading = ref(false)
const showEmpty = ref(true)

watchEffect(() => {
  document.documentElement.classList.toggle('dark', isDark.value)
})
</script>

<template>
  <div class="flex justify-center items-center h-16 fixed top-6 left-0 right-0 z-50">
    <img src="./assets/logo1.png" alt="Logo" class="w-50 opacity-100" />
  <button
      @click="isDark = !isDark"
      aria-label="Toggle dark mode"
      class="fixed top-6 right-6 z-60 group
             w-12 h-12 rounded-full flex items-center justify-center text-xl
             backdrop-blur-sm border border-white/20
             bg-white/80 text-gray-700 shadow-lg hover:shadow-xl
             dark:bg-gray-800/80 dark:text-yellow-300 dark:border-gray-700/50
             transform hover:scale-110 active:scale-95
             transition-all duration-300 ease-out
             hover:rotate-12 dark:hover:rotate-[-12deg]"
    >
      <span class="transform transition-transform duration-500 group-hover:scale-110">
        {{ isDark ? '☀️' : '🌙' }}
      </span>
    </button>
  </div>
  <div
    class="fixed inset-0 w-screen h-screen flex flex-col overflow-hidden
           transition-all duration-700 ease-in-out text-gray-900 dark:text-gray-100"
    :class="isDark 
      ? 'bg-gradient-to-br from-gray-900 via-slate-800 to-gray-900' 
      : 'bg-gradient-to-br from-blue-50 via-indigo-50 to-purple-50'"
  >
    

    <div class="fixed inset-0 overflow-hidden pointer-events-none z-0">
      <div
        :class="[
          'absolute top-20 left-20 w-3 h-3 rounded-full animate-pulse',
          isDark ? 'bg-purple-400/30' : 'bg-blue-400/20'
        ]"
        style="animation-delay: 0s; animation-duration: 3s;"
      ></div>
      <div
        :class="[
          'absolute top-1/3 right-1/4 w-2 h-2 rounded-full animate-bounce',
          isDark ? 'bg-cyan-400/40' : 'bg-indigo-400/30'
        ]"
        style="animation-delay: 1s; animation-duration: 4s;"
      ></div>
      <div
        :class="[
          'absolute bottom-1/3 left-1/5 w-4 h-4 rounded-full animate-ping',
          isDark ? 'bg-blue-400/25' : 'bg-purple-400/15'
        ]"
        style="animation-delay: 2s; animation-duration: 5s;"
      ></div>
      <div
        :class="[
          'absolute top-1/2 left-1/2 w-1 h-1 rounded-full animate-pulse',
          isDark ? 'bg-purple-400/50' : 'bg-cyan-400/40'
        ]"
        style="animation-delay: 3s; animation-duration: 6s;"
      ></div>
    </div>

    <main
      class="relative z-10 flex-1 w-full h-full
             flex flex-col justify-center items-center
             transform transition-all duration-1000 ease-out"
    >
      <div class="w-full max-w-2xl mx-auto px-8 space-y-12 animate-fadeInUp">
        <div style="animation-delay: 0.1s">
          <SearchBar :isDark="isDark" />
        </div>
        <div style="animation-delay: 0.2s">
          <LoadingView :isDark="isDark" :isLoading="isLoading" />
        </div>
        
        <div style="animation-delay: 0.3s">
          <EmptySearch :showEmpty="showEmpty" :isDark="isDark" />
        </div>
        
        <!-- Uncomment when you want to show real results -->
        <!-- <div style="animation-delay: 0.4s">
          <SearchItemList :items="dummyData" :isDark="isDark" />
        </div> -->
      </div>
    </main>

    <footer
      class="relative z-10 py-8 text-center text-sm
             backdrop-blur-sm transition-all duration-700 ease-out"
      :class="isDark 
        ? 'border-t border-gray-700/30 bg-gray-900/20' 
        : 'border-t border-white/20 bg-white/10'"
    >
      <p class="opacity-60 hover:opacity-100 transition-opacity duration-300 text-black dark:text-gray-400">
        © 2025 Books&Books
      </p>
    </footer>
  </div>
</template>

<style scoped>
@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.animate-fadeInUp {
  animation: fadeInUp 0.8s ease-out forwards;
}

.animate-fadeInUp > div {
  opacity: 0;
  animation: fadeInUp 0.6s ease-out forwards;
}

* {
  transition-property: background-color, border-color, color, fill, stroke, opacity, box-shadow, transform;
  transition-timing-function: cubic-bezier(0.4, 0, 0.2, 1);
  transition-duration: 300ms;
}

@supports (backdrop-filter: blur(10px)) {
  .backdrop-blur-sm {
    backdrop-filter: blur(4px);
  }
}
</style>