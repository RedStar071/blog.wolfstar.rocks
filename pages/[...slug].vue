<template>
	<nuxt-layout name="default">
		<ContentRenderer v-if="page" :value="page" />
		<template v-else>
			<h1>Document not found</h1>
			<p>You seem to have found a page that doesn't exist. This may be because the page has yet to be written, published, or it was removed.</p>
			<p>However, the post could exist but it's not being shown, in that case, you may need to refresh the cache.</p>
		</template>
	</nuxt-layout>
</template>

<script lang="ts" setup>
const route = useRoute();
const { data: page } = await useAsyncData(route.path, () => {
	return queryCollection('content').path(route.path).first();
});

if (page.value) {
	useHead({ title: page.value.title });
	useSeoMeta({
		title: page.value.title,
		ogTitle: page.value.title,
		description: page.value.description,
		ogDescription: page.value.description,
		ogImage: `/images/${page.value.image}.png`,
		ogImageAlt: `${page.value.image}'s avatar`,
		articleAuthor: [page.value.author],
		articlePublishedTime: page.value.date,
		articleTag: page.value.tags
	});
}
</script>
