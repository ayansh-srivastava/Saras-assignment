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
const error = ref(null)

const limit = 12
const totalFound = ref(0)
const currentPage = ref(1)
const totalPages = computed(() => Math.ceil(totalFound.value / limit))

async function fetchPage(page = 1) {
  isLoading.value = true
  showEmpty.value = false
  error.value = null

  try {
    const offset = (page - 1) * limit
    const url = `https://openlibrary.org/search.json?q=${encodeURIComponent(currentQuery.value)}&limit=${limit}&offset=${offset}`
    const res = await fetch(url)
    
    if (!res.ok) {
      throw new Error(`HTTP error! status: ${res.status}`)
    }
    
    const data = await res.json()

    totalFound.value = data.num_found
    searchResults.value = data.docs.map(mapDoc)
    isLoading.value = false
    showEmpty.value = searchResults.value.length === 0 && currentQuery.value.trim() === ''
  } catch (err) {
    console.error('Search error:', err)
    error.value = err.message || 'Failed to search books. Please try again.'
    isLoading.value = false
    showEmpty.value = false
    searchResults.value = []
    totalFound.value = 0
  }
}

function handleSearch(query) {
  error.value = null
  if (query.trim() === '') {
    showEmpty.value = true
    searchResults.value = []
    totalFound.value = 0
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
        
        
        <div v-if="error" class="text-center py-8 animate-fadeInUp">
          <div :class="[
            'inline-flex items-center px-6 py-4 rounded-xl border transition-all duration-300',
            isDark 
              ? 'bg-red-900/20 border-red-800/50 text-red-300' 
              : 'bg-red-50 border-red-200 text-red-700'
          ]">
            <svg class="w-6 h-6 mr-3 flex-shrink-0" fill="currentColor" viewBox="0 0 20 20">
              <path fill-rule="evenodd" d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-7 4a1 1 0 11-2 0 1 1 0 012 0zm-1-9a1 1 0 00-1 1v4a1 1 0 102 0V6a1 1 0 00-1-1z" clip-rule="evenodd" />
            </svg>
            <div>
              <p class="font-semibold">Search Error</p>
              <p class="text-sm opacity-90">{{ error }}</p>
            </div>
          </div>
          <button 
            @click="() => { error = null; handleSearch(currentQuery) }"
            :class="[
              'mt-4 px-4 py-2 rounded-lg font-medium transition-colors duration-300',
              isDark
                ? 'bg-red-800/30 hover:bg-red-800/50 text-red-300 border border-red-700/50'
                : 'bg-red-100 hover:bg-red-200 text-red-700 border border-red-300'
            ]">
            Try Again
          </button>
        </div>


        <div v-if="!isLoading && !error && totalFound === 0 && currentQuery.trim() !== ''" class="text-center py-12 animate-fadeInUp">
          <div :class="[
            'inline-flex flex-col items-center px-8 py-6 rounded-2xl border transition-all duration-300',
            isDark 
              ? 'bg-gray-800/50 border-gray-700/50 text-gray-300' 
              : 'bg-gray-50 border-gray-200 text-gray-600'
          ]">
            <svg class="w-16 h-16 mb-4 opacity-50" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" 
                    d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" />
            </svg>
            <h3 :class="['text-xl font-bold mb-2', isDark ? 'text-gray-200' : 'text-gray-800']">
              No Books Found
            </h3>
            <p class="text-sm opacity-75 mb-4 max-w-md">
              We couldn't find any books matching <span class="font-semibold">"{{ currentQuery }}"</span>
            </p>
            <div class="text-xs opacity-60 space-y-1">
              <p>• Try different keywords or phrases</p>
              <p>• Check your spelling</p>
              <p>• Use more general terms</p>
            </div>
          </div>
        </div>

        <EmptySearch :isDark="isDark" :showEmpty="showEmpty" />

        <SearchItemList v-if="!showEmpty && !isLoading && !error && totalFound > 0" :isDark="isDark" :items="searchResults" :totalBooks="totalFound"/>

        <div v-if="!showEmpty && !isLoading && !error && totalFound > 0" class="flex justify-center space-x-2">
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
