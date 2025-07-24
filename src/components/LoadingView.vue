<script setup>

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
</script>

<template>
  <div v-if="isLoading" class="flex items-center justify-center h-40">
    <!-- <span class="loader"></span> -->
     <div class="w-10 h-10 border-4 border-gray-300 border-t-blue-500 rounded-full animate-spin"></div>

  </div>
  <slot v-else></slot>
</template>