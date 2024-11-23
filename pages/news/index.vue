<template>
	<div class="container mx-auto px-4 py-8">
		<Navigation />
		<BreadcrumbTrail :custom-title="pageTitle" />
		<h1 class="text-4xl font-bold mb-8">{{ pageTitle }}</h1>

		<div v-if="pending" class="text-center py-8">
			<p class="text-xl text-gray-600">Loading articles...</p>
		</div>

		<div v-else-if="error" class="text-center py-8">
			<p class="text-xl text-red-600">Error loading articles. Please try again later.</p>
		</div>

		<template v-else>
			<div class="mb-6">
				<input v-model="searchQuery" type="text" placeholder="Search articles..."
					class="w-full px-4 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500" />
			</div>

			<ul v-if="filteredArticles.length" class="space-y-6">
				<li v-for="article in filteredArticles" :key="article._path" class="border-b pb-6">
					<NuxtLink :to="article._path" class="block">
						<h2 class="text-2xl font-semibold hover:text-blue-600 transition-colors">
							{{ article.title }}
						</h2>
						<p class="text-gray-600 mt-2">{{ article.description }}</p>
						<div class="flex items-center mt-2 text-sm text-gray-500">
							<span>{{ formatDate(article.date) }}</span>
							<span v-if="article.author" class="ml-4">by {{ article.author }}</span>
						</div>
					</NuxtLink>
				</li>
			</ul>

			<p v-else class="text-center py-8 text-xl text-gray-600">No articles found.</p>

			<div v-if="hasMoreArticles" class="text-center mt-8">
				<button @click="loadMoreArticles"
					class="bg-blue-500 hover:bg-blue-600 text-white font-bold py-2 px-4 rounded transition-colors">
					Load More Articles
				</button>
			</div>
		</template>
	</div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'
import BreadcrumbTrail from '~/components/BreadcrumbTrail.vue'

const pageTitle = 'News Archive'
const searchQuery = ref('')
const page = ref(1)
const perPage = 10

const { data: articles, pending, error, refresh } = await useAsyncData('articles', () =>
	queryContent('news')
		.sort({ date: -1 })
		.limit(perPage)
		.skip((page.value - 1) * perPage)
		.find()
)

const filteredArticles = computed(() => {
	if (!articles.value) return []
	return articles.value.filter(article =>
		article.title.toLowerCase().includes(searchQuery.value.toLowerCase()) ||
		article.description.toLowerCase().includes(searchQuery.value.toLowerCase())
	)
})

const hasMoreArticles = computed(() => articles.value && articles.value.length === perPage)

const loadMoreArticles = async () => {
	page.value++
	await refresh()
}

const formatDate = (date: string) => {
	return new Date(date).toLocaleDateString('en-US', {
		year: 'numeric',
		month: 'long',
		day: 'numeric'
	})
}

useHead({
	title: pageTitle,
	meta: [
		{ name: 'description', content: 'Browse our latest news articles and stay up to date with our company.' }
	]
})
</script>