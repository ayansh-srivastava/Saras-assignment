<script setup>
  import SearchBar from './components/SearchBar.vue';
  import SearchItemList from './components/SearchItemList.vue';
  import dummyData from './assets/dummyData.js';
  
  const isDark = ref(true);
  
</script>

<template>
    <div>
        <SearchBar :isDark="isDark" />
        <SearchItemList :items="dummyData" :isDark="isDark" />
      </div>
</template>


