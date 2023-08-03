<script setup>
import { onMounted, ref } from 'vue'
import axios from 'axios'
import ArticleContainer from './components/ArticleContainer.vue'
import LoadingWheel from './components/LoadingWheel.vue'

const articleList = ref([])
const errors = ref([])

onMounted(async () => {
  const date = new Date()
  date.setMonth(date.getDate() - 15)
  try {
    const response = await axios.get(
      'https://newsapi.org/v2/everything?q=sports&from=' +
        date +
        '&sortBy=publishedAt&apiKey=3ede5178a73d4c059609dc97b5dc7064'
    )
    articleList.value = response.data.articles
      .filter((a) => a.urlToImage)
      .map((a, index) => ({ ...a, id: index }))
  } catch (e) {
    errors.value.push(e)
  }
  // force ids from index as results don't have any
  // Add 'mark as viewed' property to then filter out items on click
})
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
      v-if="articleList.length > 0"
      :article-list="articleList"
      :nbElements="8"
      :scalingCoefficient="44"
    />
    <div v-else-if="errors.length > 0">
      {{ errors[0] }}
    </div>
    <LoadingWheel v-else />
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  width: 50px;
  height: 50px;
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    flex-direction: row-reverse;
    margin-bottom: 1rem;
  }

  .logo {
    margin: 0 2rem 0 0;
  }
}
</style>
