<script setup>
import { ref, computed } from 'vue'
import Layout from './layouts/Layout.vue'
import SearchBar from './components/SearchBar.vue'
import LoadingView from './components/LoadingView.vue'
import EmptySearch from './components/EmptySearch.vue'
import SearchItemList from './components/SearchItemList.vue'
import OrbBackground from './components/OrbBackground.vue'

const isLoading = ref(false)
const showEmpty = ref(true)
const searchResults = ref([])
const isDark = ref(true)
const currentQuery = ref('')

const limit = 12
const totalFound = ref(0)
const currentPage = ref(1)
const totalPages = computed(() => Math.ceil(totalFound.value / limit))

async function fetchPage(page = 1) {
  isLoading.value = true
  showEmpty.value = false

  const offset = (page - 1) * limit
  const url = `https://openlibrary.org/search.json?q=${encodeURIComponent(currentQuery.value)}&limit=${limit}&offset=${offset}`
  const res = await fetch(url)
  const data = await res.json()

  totalFound.value = data.num_found
  searchResults.value = data.docs.map(mapDoc)
  isLoading.value = false
  showEmpty.value = searchResults.value.length === 0
}

function handleSearch(query) {
  if (query.trim() === '') {
    showEmpty.value = true
    searchResults.value = []
    return
  }
  currentQuery.value = query
  currentPage.value = 1
  fetchPage(1)
}

function goToPage(page) {
  if (page < 1 || page > totalPages.value) return
  currentPage.value = page
  fetchPage(page)
}

function mapDoc(doc) {
  return {
    id: doc.key,
    item: {
      title: doc.title,
      author: doc.author_name || [],
      year: doc.first_publish_year,
      coverId: doc.cover_i
    }
  }
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

        <SearchItemList v-if="!showEmpty && !isLoading" :isDark="isDark" :items="searchResults" :totalBooks="totalFound"/>

        <div v-if="!showEmpty && !isLoading" class="flex justify-center space-x-2">
          <button class="px-4 py-2 rounded bg-indigo-600 text-white w-30 disabled:opacity-50" :disabled="currentPage === 1"
            @click="goToPage(currentPage - 1)">Previous</button>

          <span :class="['px-2 py-2', isDark ? 'text-gray-200' : 'text-gray-800']">Page {{ currentPage }} of {{ totalPages }}</span>

          <button class="px-4 py-2 rounded bg-indigo-600 text-white w-30 disabled:opacity-50"
            :disabled="currentPage === totalPages" @click="goToPage(currentPage + 1)">Next</button>
        </div>

      </div>
    </main>
  </Layout>
</template>



<style scoped>
@keyframes fadeInUp { from { opacity:0; transform:translateY(30px);} to { opacity:1; transform:translateY(0);} }
.animate-fadeInUp { animation: fadeInUp 0.8s ease-out forwards; }
</style>
