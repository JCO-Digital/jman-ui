<script setup lang="ts">
import { RouterView } from 'vue-router'
import { useData } from './composables/useData'

const { initData } = useData()
initData()
</script>

<template>
  <div class="app-container">
    <RouterView />
  </div>
</template>

<style>
/* Global styles are mostly in style.css, but we can add some app-level resets here */
html, body {
  margin: 0;
  padding: 0;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
  background-color: var(--bg-body);
  color: var(--text-main);
  min-height: 100vh;
}

.app-container {
  min-height: 100vh;
  width: 100%;
}

/* Reset some default Vite styling that might clash */
#app {
  max-width: none;
  margin: 0;
  padding: 0;
  text-align: left;
}

/* Basic resets */
* {
  box-sizing: border-box;
}

table {
  width: 100%;
  border-collapse: collapse;
}
</style>
