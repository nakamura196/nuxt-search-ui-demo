<script setup lang="ts">
import SearchSection from "./components/SearchSection.vue";
</script>
<template>
  <div>
    <h1 class="header">Search UI demo with Nuxt (Hearthstone cards)</h1>
    <ClientOnly>
      <SearchSection />
    </ClientOnly>
  </div>
</template>
<style>
@import "./node_modules/@elastic/react-search-ui-views/lib/styles/styles.css";

body {
  margin: 0;
  padding: 0;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto,
    Oxygen-Sans, Ubuntu, Cantarell, "Helvetica Neue", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}

.header {
  text-align: center;
}
</style>
