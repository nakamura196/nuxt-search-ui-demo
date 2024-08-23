<script setup lang="ts">
import { ref, watch } from "vue";

// Use `defineProps` to define props that this component accepts
const props = defineProps({
  modelValue: Number,
});

// Use `emit` to handle emitting custom events
const emit = defineEmits(["update:modelValue"]);

// Bind the model to a local reactive state
const model = ref(props.modelValue);

// Watch for changes in `model` and emit the updated value
watch(model, (newValue) => {
  emit("update:modelValue", newValue);
});
</script>

<template>
  <div class="sui-results-per-page">
    <div class="sui-results-per-page__label">Show</div>
    <select name="resultsPerPage" v-model="model">
      <option :value="20">20</option>
      <option :value="40">40</option>
      <option :value="60">60</option>
    </select>
  </div>
</template>
