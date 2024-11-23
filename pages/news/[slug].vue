<template>
	<div class="bg-gray-50 min-h-screen">
		<Navigation />
		<div class="container mx-auto px-4 py-8">
			<BreadcrumbTrail :custom-title="article?.title" />

			<main class="bg-white shadow-lg rounded-lg p-6 md:p-8 mt-6">
				<article v-if="article">
					<header class="mb-8">
						<h1 class="text-4xl font-bold text-gray-900 mb-4">{{ article.title }}</h1>
						<div class="flex items-center text-sm text-gray-600">
							<time :datetime="article.date">{{ formatDate(article.date) }}</time>
							<span v-if="article.author" class="mx-2">•</span>
							<span v-if="article.author">{{ article.author }}</span>
						</div>
					</header>

					<div class="prose prose-lg max-w-none">
						<ContentRenderer :value="article" />
					</div>

					<footer class="mt-8 pt-8 border-t border-gray-200">
						<div v-if="article.tags" class="flex flex-wrap gap-2">
							<span v-for="tag in article.tags" :key="tag"
								class="bg-blue-100 text-blue-800 text-sm font-medium px-2.5 py-0.5 rounded">
								{{ tag }}
							</span>
						</div>
					</footer>
				</article>

				<div v-else-if="pending" class="text-center py-12">
					<p class="text-xl text-gray-600">Loading article...</p>
				</div>

				<div v-else class="text-center py-12">
					<p class="text-xl text-red-600">Article not found.</p>
				</div>
			</main>

			<aside class="mt-12">
				<h2 class="text-2xl font-bold text-gray-900 mb-4">Related Articles</h2>
				<div v-if="relatedArticles.length" class="grid gap-6 md:grid-cols-2 lg:grid-cols-3">
					<NuxtLink v-for="relatedArticle in relatedArticles" :key="relatedArticle._path" :to="relatedArticle._path"
						class="bg-white shadow-md rounded-lg overflow-hidden hover:shadow-lg transition-shadow duration-300">
						<div class="p-6">
							<h3 class="text-xl font-semibold text-gray-900 mb-2">{{ relatedArticle.title }}</h3>
							<p class="text-gray-600 mb-4">{{ relatedArticle.description }}</p>
							<time :datetime="relatedArticle.date" class="text-sm text-gray-500">
								{{ formatDate(relatedArticle.date) }}
							</time>
						</div>
					</NuxtLink>
				</div>
				<p v-else class="text-center text-gray-600">No related articles found.</p>
			</aside>
		</div>
	</div>
</template>

<script setup lang="ts">
import { useRoute } from 'vue-router'
import BreadcrumbTrail from '~/components/BreadcrumbTrail.vue'

const route = useRoute()
const slug = Array.isArray(route.params.slug) ? route.params.slug.join('/') : route.params.slug

const { data: article, pending } = await useAsyncData(`article-${slug}`, () =>
	queryContent('news', slug).findOne()
)

const { data: relatedArticles } = await useAsyncData(`related-articles-${slug}`, () =>
	queryContent('news')
		.where({ _path: { $ne: `/news/${slug}` } })
		.limit(3)
		.find()
)

const formatDate = (date: string) => {
	return new Date(date).toLocaleDateString('en-US', {
		year: 'numeric',
		month: 'long',
		day: 'numeric'
	})
}

useHead(() => ({
	title: article.value?.title,
	meta: [
		{ name: 'description', content: article.value?.description || 'Article page' },
		{ property: 'og:title', content: article.value?.title },
		{ property: 'og:description', content: article.value?.description },
		{ name: 'twitter:title', content: article.value?.title },
		{ name: 'twitter:description', content: article.value?.description },
	]
}))
</script>