<script setup>
import { defineProps } from 'vue'

const props = defineProps({
    item: {
        type: Object,
        required: true
    },
    isDark: {
        type: Boolean,
        default: true
    }
})
</script>
<template>
  <div class="p-4 border rounded-lg shadow-sm hover:shadow-md transition-shadow duration-200">
    <h3 class="text-lg font-semibold">{{ item.title }}</h3>
    <p class="text-gray-600">{{ item.author }}</p>
    <p class="text-gray-500 text-sm">{{ item.description }}</p>
  </div>
</template>