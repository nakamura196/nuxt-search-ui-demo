<script setup lang="ts">
import { ref, watch } from "vue";

// Use `defineProps` to define props that this component accepts
const props = defineProps({
  modelValue: String,
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
  <div class="sui-sorting">
    <div class="sui-sorting__label">Sort by</div>
    <select name="sortBy" v-model="model">
      <option value="">Relevance</option>
      <option value="name">Name</option>
    </select>
  </div>
</template>
