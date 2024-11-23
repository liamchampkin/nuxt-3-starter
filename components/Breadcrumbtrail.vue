<template>
	<nav aria-label="Breadcrumb" class="breadcrumb-nav">
		<ol vocab="http://schema.org/" typeof="BreadcrumbList" class="breadcrumb-list">
			<li property="itemListElement" typeof="ListItem" class="breadcrumb-item">
				<NuxtLink property="item" typeof="WebPage" to="/" class="home-link">
					<span property="name">Home</span>
				</NuxtLink>
				<meta property="position" content="1" />
			</li>
			<li v-for="(crumb, index) in breadcrumbs" :key="crumb.path" property="itemListElement" typeof="ListItem"
				class="breadcrumb-item">
				<NuxtLink property="item" typeof="WebPage" :to="crumb.path" :class="{ 'is-active': isCurrentPage(crumb.path) }">
					<span property="name">{{
						isCurrentPage(crumb.path) && customTitle ? customTitle : crumb.title
					}}</span>
				</NuxtLink>
				<meta property="position" :content="index + 2" />
			</li>
		</ol>
	</nav>
</template>

<script setup lang="ts">
import { computed } from 'vue'
import { useRoute } from '#app'

interface BreadcrumbItem {
	title: string
	path: string
}

interface Props {
	customTitle?: string
}

const props = withDefaults(defineProps<Props>(), {
	customTitle: undefined
})

const route = useRoute()

const formatTitle = (text: string): string => {
	return text
		.split('-')
		.map(word => word.charAt(0).toUpperCase() + word.slice(1))
		.join(' ')
}

const isCurrentPage = (path: string): boolean => {
	return route.path === path
}

const breadcrumbs = computed<BreadcrumbItem[]>(() => {
	const pathSegments = route.path.split('/').filter(segment => segment)
	const crumbs: BreadcrumbItem[] = []
	let currentPath = ''

	pathSegments.forEach((segment) => {
		currentPath += `/${segment}`
		crumbs.push({
			title: formatTitle(segment),
			path: currentPath
		})
	})

	return crumbs
})
</script>

<style scoped>
.breadcrumb-nav {
	padding: 1rem 0;
}

.breadcrumb-list {
	list-style: none;
	padding: 0;
	margin: 0;
	display: flex;
	flex-wrap: wrap;
	gap: 0.5rem;
}

.breadcrumb-item {
	display: flex;
	align-items: center;
	color: #666;
	font-size: 0.875rem;
}

.breadcrumb-item:not(:last-child)::after {
	content: '›';
	margin-left: 0.5rem;
	color: #999;
}

.breadcrumb-item a {
	color: #2563eb;
	text-decoration: none;
	transition: color 0.2s ease;
}

.breadcrumb-item a:hover {
	color: #1d4ed8;
	text-decoration: underline;
}

.breadcrumb-item a.is-active {
	color: #666;
	pointer-events: none;
}

.home-link {
	display: inline-flex;
	align-items: center;
	gap: 0.25rem;
}
</style>