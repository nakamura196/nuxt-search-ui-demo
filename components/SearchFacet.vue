<script setup lang="ts">
import type {
  Facet,
  FacetType,
  FacetValue,
  FilterValue,
  FilterValueRange,
} from "@elastic/search-ui";

interface PropType {
  facet: Facet | null;
  checked: Array<FilterValue>;
}

const Props = withDefaults(defineProps<PropType>(), {
  facet: null,
  checked: () => [],
});

const isChecked = (value: FilterValue) => {
  return Props.checked && Props.checked.includes(value);
};

const getValue = (facetItem: FacetValue, type: FacetType) => {
  return type === "range"
    ? (facetItem.value as FilterValueRange).name
    : facetItem.value;
};
</script>
<template>
  <div
    v-if="facet"
    v-show="facet.data.length"
    class="sui-multi-checkbox-facet sui-facet"
  >
    <div class="sui-multi-checkbox-facet__label">
      {{ facet.field }}
    </div>
    <div class="sui-multi-checkbox-facet__options-list">
      <label
        v-for="facetItem in facet.data"
        :key="String(getValue(facetItem, facet.type))"
        class="sui-multi-checkbox-facet__option-label"
      >
        <div class="sui-multi-checkbox-facet__option-input-wrapper">
          <input
            class="sui-multi-checkbox-facet__checkbox"
            type="checkbox"
            :value="getValue(facetItem, facet.type)"
            :checked="isChecked(getValue(facetItem, facet.type))"
            @change="$emit('change', $event)"
          />
          <span class="sui-multi-checkbox-facet__input-text">{{
            getValue(facetItem, facet.type)
          }}</span>
        </div>
        <span class="sui-multi-checkbox-facet__option-count">{{
          facetItem.count
        }}</span>
      </label>
    </div>
  </div>
</template>
