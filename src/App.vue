<script setup lang="ts">
import { RouterView } from "vue-router";
import { useData } from "./composables/useData";
import packageInfo from "../package.json";

const { initData } = useData();
initData();

const version = packageInfo.version;
</script>

<template>
	<div class="app-container">
		<div class="main-content">
			<RouterView />
		</div>
		<footer class="app-footer">v{{ version }}</footer>
	</div>
</template>

<style>
/* Global styles are mostly in style.css, but we can add some app-level resets here */
html,
body {
	margin: 0;
	padding: 0;
	font-family:
		-apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial,
		sans-serif;
	background-color: var(--bg-body);
	color: var(--text-main);
	min-height: 100vh;
}

.app-container {
	min-height: 100vh;
	width: 100%;
	display: flex;
	flex-direction: column;
}

.main-content {
	flex: 1;
	display: flex;
	flex-direction: column;
}

.app-footer {
	margin-top: auto;
	padding: 1rem;
	text-align: center;
	font-size: 0.875rem;
	color: var(--text-muted, #888);
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
