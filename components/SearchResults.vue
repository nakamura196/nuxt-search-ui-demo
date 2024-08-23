<script setup lang="ts">
import type { SearchResult } from "@elastic/search-ui";
import SearchEachResult from "./SearchResult.vue";
interface PropType {
  results: Array<SearchResult>;
}

withDefaults(defineProps<PropType>(), {
  results: () => [],
});
</script>
<template>
  <ul class="sui-results-container search-section__search-results">
    <li
      v-for="result in results"
      :key="result.id.raw"
      class="search-section__search-result"
    >
      <SearchEachResult :result="result" />
    </li>
  </ul>
</template>
<style>
.search-section__search-results {
  display: flex;
  flex-wrap: wrap;
}

.search-section__search-result {
  width: 48%;
  margin: 1%;
  background: white;
}
</style>
