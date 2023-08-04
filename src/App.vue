<script setup>
import { onMounted, ref } from 'vue'
import axios from 'axios'
import ArticleContainer from './components/ArticleContainer.vue'
import LoadingWheel from './components/LoadingWheel.vue'
import RangeSlider from './components/RangeSlider.vue'

const articleList = ref([])
const errors = ref([])
const nbElements = ref(8)
const scalingCoefficient = ref(44)

onMounted(async () => {
  const today = new Date()
  const sinceDate = new Date()
  sinceDate.setDate(today.getDate() - 15)
  try {
    const response = await axios.get(
      'https://api.nytimes.com/svc/topstories/v2/world.json?api-key=uKBGQUvgMqVgFX7JdVv4Ilx5D2e1efo8'
    )
    articleList.value = response.data.results
      .filter((a) => a.multimedia[0].url.includes('.jpg'))
      .map((a, index) => ({ ...a, id: index, image: a.multimedia[0].url })) // Force ids from index
  } catch (e) {
    errors.value.push(e)
  }
})
</script>

<template>
  <header>
    <div class="input-container">
      <RangeSlider
        id="article-number"
        name="Article number"
        value="8"
        :min="0"
        :max="16"
        v-model="nbElements"
      />
      <RangeSlider
        id="scaling-coefficient"
        name="Scaling coefficient"
        value="8"
        :min="1"
        :max="100"
        v-model="scalingCoefficient"
      />
    </div>
    <div class="wrapper">
      <h1>Fibonacci News</h1>
      <img
        alt="Vue logo"
        class="logo"
        src="./assets/logo.svg"
        width="125"
        height="125"
      />
    </div>
  </header>

  <main>
    <ArticleContainer
      v-if="articleList.length > 0"
      :article-list="articleList"
      :nbElements="nbElements"
      :scalingCoefficient="scalingCoefficient"
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
header .wrapper {
  display: flex;
}

.logo {
  width: 50px;
  height: 50px;
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    margin: 0.5rem 2rem 1rem 2rem;
    display: flex;
    justify-content: space-between;
    margin-bottom: 1rem;
  }

  .logo {
    margin: 0 2rem 0 0;
  }
}
.input-container {
  display: flex;
  gap: 2rem;
}
</style>
