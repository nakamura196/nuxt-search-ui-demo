<script setup lang="ts">
import type { SearchState } from "@elastic/search-ui";
interface PropType {
  searchState: SearchState;
}

const Props = withDefaults(defineProps<PropType>(), {
  searchState: () => ({} as SearchState),
});

const start = computed(() => {
  return Props.searchState.totalResults === 0
    ? 0
    : ((Props.searchState.current || 0) - 1) *
        (Props.searchState.resultsPerPage || 0) +
        1;
});

const end = computed(() => {
  return Props.searchState.resultsPerPage &&
    Props.searchState.totalResults <= Props.searchState.resultsPerPage
    ? Props.searchState.totalResults
    : start.value * (Props.searchState.resultsPerPage || 0);
});
</script>
<template>
  <div class="sui-paging-info">
    Showing
    <strong>{{ start }} - {{ Math.min(end, searchState.totalResults) }}</strong>
    out of <strong>{{ searchState.totalResults }}</strong> for:
    <em>"{{ searchState.searchTerm }}"</em>
  </div>
</template>
