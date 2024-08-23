<script setup lang="ts">
import { ref, watch } from "vue";
import { defineProps, defineEmits } from "vue";

// Use `defineProps` to define props that this component accepts
const props = defineProps({
  modelValue: String,
});

// Use `emit` to handle emitting custom events
const emit = defineEmits(["update:modelValue", "submit"]);

const searchInputValue = ref(props.modelValue);

// Sync the `searchInputValue` with `modelValue` prop
watchEffect(() => {
  searchInputValue.value = props.modelValue;
});

// Watch for changes in `searchInputValue` and emit the updated value
watch(searchInputValue, (newValue) => {
  emit("update:modelValue", newValue);
});

const handleFormSubmit = () => {
  emit("submit", searchInputValue.value);
};
</script>
<template>
  <div class="sui-layout-header">
    <div class="sui-layout-header__inner">
      <form @submit.prevent="handleFormSubmit">
        <div class="sui-search-box">
          <div class="sui-search-box__wrapper">
            <input
              type="text"
              placeholder="Try searching for 'water', 'dragon' or 'ragnaros'"
              class="sui-search-box__text-input"
              v-model="searchInputValue"
            />
          </div>
          <input
            type="submit"
            class="button sui-search-box__submit"
            value="Search"
          />
        </div>
      </form>
    </div>
  </div>
</template>
