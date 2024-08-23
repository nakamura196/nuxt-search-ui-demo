<script setup lang="ts">
import { SearchDriver } from "@elastic/search-ui";
import type {
  SearchState,
  FacetValue,
  FilterValue,
  FilterValueRange,
  FacetConfiguration,
} from "@elastic/search-ui";
import SearchPagingInfo from "@/components/SearchPagingInfo.vue";
import SearchSort from "@/components/SearchSort.vue";
import SearchPagination from "@/components/SearchPagination.vue";
import SearchFacet from "@/components/SearchFacet.vue";
import SearchHeader from "@/components/SearchHeader.vue";
import SearchResults from "@/components/SearchResults.vue";
import SearchResultsPerPage from "@/components/SearchResultsPerPage.vue";
import config from "@/searchConfig";

const driver = new SearchDriver(config);

const searchState = ref<SearchState>(driver.getState());
const searchInputValue = ref<string>(searchState.value.searchTerm || "");
const defaultResultsPerPage = 20;
const resultsPerPage_ = ref<number>(
  searchState.value.resultsPerPage || defaultResultsPerPage
);
const defaultSortBy = "";
const sortBy = ref<string>(searchState.value.sortField || defaultSortBy);
const card_class = ref<FilterValue[]>([]);
const race = ref<FilterValue[]>([]);
const rarity = ref<FilterValue[]>([]);
const type = ref<FilterValue[]>([]);
const set = ref<FilterValue[]>([]);
const cost = ref<FilterValue[]>([]);

const fields: {
  [key: string]: Ref<FilterValue[]>;
} = {
  card_class,
  race,
  rarity,
  type,
  set,
  cost,
};

onMounted(() => {
  const { searchTerm, sortField, resultsPerPage, filters } = driver.getState();

  searchInputValue.value = searchTerm || "";

  sortBy.value = sortField || "";
  resultsPerPage_.value = resultsPerPage || 20;

  filters?.forEach((filter) => {
    const facets_ = config.searchQuery.facets as {
      [key: string]: FacetConfiguration;
    };
    const field = filter.field;
    if (facets_[field].type === "range") {
      fields[filter.field].value = filter.values.map(
        (value) => (value as FilterValueRange).name
      );
    } else {
      fields[filter.field].value = filter.values;
    }
  });

  driver.subscribeToStateChanges((state) => {
    searchState.value = state;
  });
});

const setCurrentPage = (page: number) => {
  driver.setCurrent(page);
};

const handleFacetChange = (
  event: {
    target: { value: FilterValue; checked: boolean };
  },
  facet: string
) => {
  const { value, checked } = event.target;
  const facetFromDriver = driver.getState().facets[facet][0];
  const valueforApi =
    facetFromDriver.type === "range"
      ? facetFromDriver.data.find(
          (item: FacetValue) => (item.value as FilterValueRange).name === value
        ).value
      : value;

  if (checked) {
    fields[facet].value.push(value);
    driver.addFilter(facet, valueforApi, "any");
  } else {
    const index = fields[facet].value.indexOf(value);
    if (index > -1) {
      fields[facet].value.splice(index, 1);
    }
    driver.removeFilter(facet, valueforApi, "any");
  }
};

const handleFormSubmit = () => {
  driver.getActions().setSearchTerm(searchInputValue.value);
};

const thereAreResults = computed(() => {
  return searchState.value.totalResults && searchState.value.totalResults > 0;
});

watch(resultsPerPage_, (newResultsPerPage) => {
  driver.getActions().setResultsPerPage(newResultsPerPage);
});

watch(sortBy, (newSortBy) => {
  driver.getActions().setSort(newSortBy, "asc");
});
</script>
<template>
  <div class="sui-layout">
    <SearchHeader v-model="searchInputValue" @submit="handleFormSubmit" />
    <div v-if="searchState.wasSearched" class="sui-layout-body">
      <div class="sui-layout-body__inner">
        <div class="sui-layout-sidebar">
          <SearchSort v-show="thereAreResults" v-model="sortBy" />

          <SearchFacet
            :checked="cost"
            :facet="searchState.facets.cost[0]"
            @change="handleFacetChange($event, 'cost')"
          />

          <SearchFacet
            :checked="card_class"
            :facet="searchState.facets.card_class[0]"
            @change="handleFacetChange($event, 'card_class')"
          />

          <SearchFacet
            :checked="type"
            :facet="searchState.facets.type[0]"
            @change="handleFacetChange($event, 'type')"
          />

          <SearchFacet
            :checked="set"
            :facet="searchState.facets.set[0]"
            @change="handleFacetChange($event, 'set')"
          />

          <SearchFacet
            :checked="race"
            :facet="searchState.facets.race[0]"
            @change="handleFacetChange($event, 'race')"
          />

          <SearchFacet
            :checked="rarity"
            :facet="searchState.facets.rarity[0]"
            @change="handleFacetChange($event, 'rarity')"
          />
        </div>
        <div class="sui-layout-main">
          <div class="sui-layout-main-header">
            <div class="sui-layout-main-header__inner">
              <SearchPagingInfo :search-state="searchState" />

              <SearchResultsPerPage
                v-show="thereAreResults"
                v-model.number="resultsPerPage_"
              />
            </div>
          </div>
          <div class="sui-layout-main-body">
            <SearchResults
              v-show="thereAreResults"
              :results="searchState.results"
            />
          </div>
          <div class="sui-layout-main-footer">
            <SearchPagination
              v-show="thereAreResults"
              :total-pages="Math.min(searchState.totalPages, 100)"
              :click-handler="setCurrentPage"
              :current-page="searchState.current"
            />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
