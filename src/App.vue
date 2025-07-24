<script setup>
  import SearchBar from './components/SearchBar.vue';
  import SearchItemList from './components/SearchItemList.vue';
  import dummyData from './assets/dummyData.js';
  import LoadingView from './components/LoadingView.vue';
  import { ref } from 'vue';
import EmptySearch from './components/EmptySearch.vue';
  const isDark = ref(true);
  const showEmpty = ref(false);
</script>

<template>
    <div>
          <SearchBar :isDark="isDark" />
        <!-- <SearchItemList :items="dummyData" :isDark="isDark" /> -->
         <LoadingView :isDark="isDark" :isLoading="true">
           </LoadingView>
           <EmptySearch :showEmpty="showEmpty"></EmptySearch>
      </div>
</template>


