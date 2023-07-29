<script setup>
import { onMounted, ref, computed } from 'vue'
import axios from 'axios'

const articles = ref([])

onMounted(async () => {
  let result = await axios.get(
    'https://newsapi.org/v2/everything?q=sports&from=2023-07-01&sortBy=publishedAt&apiKey=3ede5178a73d4c059609dc97b5dc7064'
  )
  articles.value = result.data.articles.filter((a) => a.urlToImage)
  articles.value.forEach((a) => {
    a.isMarkedAsViewed = false
  })
})

const notViewedArticles = computed(() => {
  console.log(articles.value.filter((a) => !a.isMarkedAsViewed).slice(0, 10))
  return articles.value.filter((a) => !a.isMarkedAsViewed).slice(0, 10)
})

function markAsViewed(index) {
  console.log('mark')
  articles.value[index].isMarkedAsViewed = true
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
    <ul>
      <li v-for="(article, index) in notViewedArticles" :key="index">
        {{ article.title }}
        <button @click="markAsViewed(index)">Mark as viewed</button>
      </li>
    </ul>
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
    justify-content: flex-start;
  }

  .logo {
    margin: 0 2rem 0 0;
  }
}
</style>
