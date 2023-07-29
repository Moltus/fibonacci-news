<script setup>
import { onMounted, ref } from 'vue'
import axios from 'axios'
import ArticleContainer from './components/ArticleContainer.vue'

const articleList = ref([])

onMounted(async () => {
  let result = await axios.get(
    'https://newsapi.org/v2/everything?q=sports&from=2023-07-01&sortBy=publishedAt&apiKey=3ede5178a73d4c059609dc97b5dc7064'
  )
  // force ids from index as results don't have any
  // Add 'mark as viewed' property to then filter out items on click
  articleList.value = result.data.articles
    .filter((a) => a.urlToImage)
    .map((a, index) => ({ ...a, id: index, isMarkedAsViewed: false }))
})

function markAsViewed(id) {
  const article = articleList.value.find((a) => a.id === id)
  if (!article) {
    throw new Error('Cannot find article with this id')
  }
  article.isMarkedAsViewed = true
}
</script>

<template>
  <header>
    <img
      alt="Vue logo"
      class="logo"
      src="./assets/logo.svg"
      width="125"
      height="125"
    />

    <div class="wrapper">
      <h1>Fibonacci News</h1>
    </div>
  </header>

  <main>
    <ArticleContainer
      :article-list="articleList"
      @handle-click-on-viewed="markAsViewed"
    />
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    flex-direction: row-reverse;
    justify-content: flex-end;
  }

  .logo {
    margin: 0 2rem 0 0;
  }
}

.article-container {
  width: 22rem;
  height: 22rem;
  display: grid;
  grid-template: repeat(3, 1fr) / repeat(3, 1fr);
  gap: 1rem 1rem;
}

.article-container li {
  width: 20rem;
  height: 20rem;
  list-style: none;
}
</style>
