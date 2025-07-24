<script setup>
import { defineProps } from 'vue'
import SearchItem from './SearchItem.vue'

const props = defineProps({
  items: {
    type: Array,
    required: true
  },
  isDark: {
    type: Boolean,
    default: true
  }
})
</script>

<template>
  <div class="grid grid-cols-1 gap-4">
    <SearchItem
      v-for="item in items"
      :key="item.id"
      :item="item"
      :isDark="isDark"
    />
  </div>
</template>
