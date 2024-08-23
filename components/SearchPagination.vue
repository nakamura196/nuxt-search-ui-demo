<script setup lang="ts">
interface PropType {
  clickHandler: (page: number) => void;
  totalPages: number;
  currentPage: number;
}

withDefaults(defineProps<PropType>(), {
  totalPages: 0,
});
</script>
<template>
  <div>
    <button
      v-for="page in totalPages"
      :key="page"
      @click="clickHandler(page)"
      style="
        margin: 5px;
        padding: 5px 10px;
        border: 1px solid #ccc;
        border-radius: 5px;
        background-color: #fff;
        cursor: pointer;
      "
      :style="currentPage === page ? { backgroundColor: '#ccc' } : {}"
    >
      {{ page }}
    </button>
  </div>
</template>
