<script setup lang="ts">
import { computed } from "vue";
import { useRouter } from "vue-router";
import { useData } from "../composables/useData";

const props = defineProps<{
	id: string;
}>();

const router = useRouter();
const { getSiteById, getServerById, getPluginsBySiteId, isLoading } = useData();

const siteId = parseInt(props.id, 10);
const site = getSiteById(siteId);
const server = computed(() =>
	site.value ? getServerById(site.value.server_id).value : null,
);
const sitePlugins = getPluginsBySiteId(siteId);

const goBack = () => {
	router.push({ name: "home" });
};
</script>

<template>
	<div class="site-detail-view">
		<header class="header">
			<div class="title-area">
				<button class="back-btn" @click="goBack">&larr; Back to Sites</button>
				<h1>Site Details</h1>
			</div>
		</header>

		<main class="content" v-if="site">
			<section class="card">
				<h2>Site Information</h2>
				<div class="info-grid">
					<div class="info-item">
						<span class="label">Site ID:</span>
						<span class="value">{{ site.id }}</span>
					</div>
					<div class="info-item">
						<span class="label">Domain:</span>
						<span class="value">{{ site.domain }}</span>
					</div>
					<div class="info-item">
						<span class="label">PHP Version:</span>
						<span class="value">{{ site.php_version }}</span>
					</div>
					<div class="info-item">
						<span class="label">Status:</span>
						<span class="value">{{ site.status }}</span>
					</div>
				</div>
			</section>

			<section class="card" v-if="server">
				<h2>Server Information</h2>
				<div class="info-grid">
					<div class="info-item">
						<span class="label">Server Name:</span>
						<span class="value">{{ server.name }}</span>
					</div>
					<div class="info-item">
						<span class="label">IP Address:</span>
						<span class="value">{{ server.ip_address }}</span>
					</div>
				</div>
			</section>

			<section class="card">
				<h2>Installed Plugins ({{ sitePlugins.length }})</h2>
				<div class="table-container">
					<table class="plugin-table">
						<thead>
							<tr>
								<th>Plugin Name</th>
								<th>Version</th>
								<th>Status</th>
							</tr>
						</thead>
						<tbody>
							<tr v-if="sitePlugins.length === 0">
								<td colspan="3" class="empty-state">No plugins found.</td>
							</tr>
							<tr v-for="plugin in sitePlugins" :key="plugin.name">
								<td>{{ plugin.name }}</td>
								<td>{{ plugin.version }}</td>
								<td>
									<span :class="['status-badge', plugin.status.toLowerCase()]">
										{{ plugin.status }}
									</span>
								</td>
							</tr>
						</tbody>
					</table>
				</div>
			</section>
		</main>
		<main class="content" v-else>
			<div class="card">
				<div v-if="isLoading" class="empty-state">
					<div class="spinner" style="margin-bottom: 12px"></div>
					<div>Loading site details...</div>
				</div>
				<div v-else class="empty-state">
					<p>Site not found.</p>
					<button class="back-btn" @click="goBack" style="margin-top: 16px">
						Go back to sites
					</button>
				</div>
			</div>
		</main>
	</div>
</template>

<style scoped>
.site-detail-view {
	max-width: 1000px;
	width: 95%;
	margin: 0 auto;
	padding: 20px;
}

.header {
	margin-bottom: 24px;
}

.title-area {
	display: flex;
	align-items: center;
	gap: 16px;
}

.title-area h1 {
	margin: 0;
	font-size: 24px;
}

.back-btn {
	padding: 8px 12px;
	background-color: transparent;
	border: 1px solid var(--border-input);
	border-radius: 4px;
	cursor: pointer;
	color: var(--text-main);
	font-weight: 500;
}

.back-btn:hover {
	background-color: var(--bg-hover);
}

.content {
	display: flex;
	flex-direction: column;
	gap: 24px;
}

.card {
	background: var(--bg-card);
	border: 1px solid var(--border-color);
	border-radius: 8px;
	padding: 20px;
}

.card h2 {
	margin-top: 0;
	margin-bottom: 16px;
	font-size: 18px;
	color: var(--text-heading);
	border-bottom: 1px solid var(--border-color);
	padding-bottom: 8px;
}

.info-grid {
	display: grid;
	grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
	gap: 16px;
}

.info-item {
	display: flex;
	flex-direction: column;
	gap: 4px;
}

.label {
	font-size: 12px;
	text-transform: uppercase;
	color: var(--text-muted);
	font-weight: 600;
	letter-spacing: 0.05em;
}

.value {
	font-size: 16px;
	color: var(--text-heading);
}

.placeholder {
	color: var(--text-placeholder);
	font-style: italic;
}

.table-container {
	border: 1px solid var(--border-color);
	border-radius: 6px;
	overflow: hidden;
}

.plugin-table {
	width: 100%;
	border-collapse: collapse;
	text-align: left;
}

.plugin-table th,
.plugin-table td {
	padding: 10px 16px;
	border-bottom: 1px solid var(--border-color);
}

.plugin-table th {
	background-color: var(--bg-table-header);
	font-weight: 600;
	color: var(--text-table-header);
}

.plugin-table tbody tr:last-child td {
	border-bottom: none;
}

.empty-state {
	text-align: center;
	padding: 24px !important;
	color: var(--text-muted);
}

.status-badge {
	display: inline-block;
	padding: 2px 8px;
	border-radius: 9999px;
	font-size: 12px;
	font-weight: 500;
	text-transform: capitalize;
	background-color: var(--badge-default-bg);
	color: var(--badge-default-text);
}

.status-badge.active {
	background-color: var(--badge-active-bg);
	color: var(--badge-active-text);
}

.status-badge.inactive,
.status-badge.disabled {
	background-color: var(--badge-inactive-bg);
	color: var(--badge-inactive-text);
}

.status-badge.must-use {
	background-color: var(--badge-must-use-bg);
	color: var(--badge-must-use-text);
}

.status-badge.drop-in {
	background-color: var(--badge-drop-in-bg);
	color: var(--badge-drop-in-text);
}
</style>
