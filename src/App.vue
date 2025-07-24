<script setup>
import Layout from './layouts/Layout.vue'
import SearchBar from './components/SearchBar.vue'
import LoadingView from './components/LoadingView.vue'
import EmptySearch from './components/EmptySearch.vue'
import SearchItemList from './components/SearchItemList.vue'

import { ref } from 'vue'
import OrbBackground from './components/OrbBackground.vue'

const isLoading = ref(false)
const showEmpty = ref(true)
const searchResults = ref([])
const isDark = ref(true)


const handleSearch = async (query) => {
  isLoading.value = true
  showEmpty.value = false
  if (query.trim() === '') {
    isLoading.value = false
    showEmpty.value = true
    return
  }
  const url = `https://openlibrary.org/search.json?q=${encodeURIComponent(query)}&limit=10`;
  const res = await fetch(url)
  const data = await res.json()
  isLoading.value = false
  showEmpty.value = data.docs.length === 0
  searchResults.value = data.docs.map(doc => ({
    id: doc.key,
    item: {
      title: doc.title,
      author: doc.author_name || [],
      year: doc.first_publish_year,
      coverId: doc.cover_i
    }
  }))
}
</script>

<template>
  <OrbBackground />
  <Layout :isDark="isDark" @toggle-theme="isDark = !isDark">
    <main class="w-full pt-24 pb-20 flex-1 flex flex-col items-center relative z-10">
      <div class="w-full mx-auto px-6 space-y-8 animate-fadeInUp">
        <SearchBar :isDark="isDark" @search="handleSearch" />
        <LoadingView :isDark="isDark" :isLoading="isLoading" />
        <EmptySearch :isDark="isDark" :showEmpty="showEmpty" />
        <div v-if="!showEmpty && !isLoading">
          <SearchItemList :isDark="isDark" :items="searchResults" />
        </div>
      </div>
    </main>
  </Layout>
</template>
