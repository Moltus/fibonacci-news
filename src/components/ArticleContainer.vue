<script setup>
import ArticleItem from './ArticleItem.vue'
import { computed } from 'vue'

defineEmits(['handleClickOnViewed'])
const props = defineProps(['articleList'])

const unreadArticles = computed(() => {
  return props.articleList
    .filter(
      (a) => a.isMarkedAsViewed !== undefined && a.isMarkedAsViewed === false
    )
    .slice(0, 9)
})
</script>

<template>
  <ArticleItem
    v-for="article in unreadArticles"
    :key="article.id"
    :content="article"
  />
</template>
