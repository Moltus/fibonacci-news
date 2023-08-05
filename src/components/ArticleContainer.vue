<script setup>
import ArticleItem from './ArticleItem.vue'
import ArrowButton from './ArrowButton.vue'
import { computed, ref, onMounted } from 'vue'

const props = defineProps(['articleList', 'nbElements', 'scalingCoefficient'])

const currentArticle = ref(0)

onMounted(() => {
  currentArticle.value = props.nbElements
})

const sequenceTemplates = computed(() => {
  const templates = []
  templates[0] = {}
  templates[0].size =
    templates[0].fontSize =
    templates[0].top =
    templates[0].left =
      0

  templates[1] = {}
  templates[1].size = 1
  templates[1].fontSize = 1 / 3
  templates[1].top = templates[1].left = 0

  for (let i = 2; i < props.nbElements; i++) {
    let itemSize = templates[i - 1].size + templates[i - 2].size
    let previousElement = templates[i - 1]

    templates[i] = {}
    templates[i].size = itemSize
    templates[i].fontSize = itemSize / 3
    if (i % 4 === 0) {
      // bottom left anchor
      templates[i].top = previousElement.top + previousElement.size
      templates[i].left = previousElement.left
    } else if (i % 4 === 1) {
      // bottom right anchor
      templates[i].top = previousElement.top + previousElement.size - itemSize
      templates[i].left = previousElement.left + previousElement.size
    } else if (i % 4 === 2) {
      // top right anchor
      templates[i].top = previousElement.top - itemSize
      templates[i].left = previousElement.left + previousElement.size - itemSize
    } else if (i % 4 === 3) {
      // top left anchor
      templates[i].top = previousElement.top
      templates[i].left = previousElement.left - itemSize
    }
  }
  return templates
})

const displayedArticles = computed(() => {
  return sequenceTemplates.value.map((element, index) => {
    return {
      ...element,
      title: props.articleList[currentArticle.value + index]?.title,
      image: props.articleList[currentArticle.value + index]?.image,
      url: props.articleList[currentArticle.value + index]?.url,
      id: props.articleList[currentArticle.value + index]?.id
    }
  })
})

const cycleArticle = (reverse = false) => {
  if (reverse) {
    currentArticle.value =
      (currentArticle.value + 1) % (props.articleList.length - props.nbElements)
  } else {
    let next = --currentArticle.value
    if (next < 0) next = props.articleList.length - props.nbElements
    currentArticle.value = next
  }
}
</script>

<template>
  <div
    class="article-container"
    :style="{
      left:
        'calc(' +
        Math.min(
          sequenceTemplates.at(-1)?.left,
          sequenceTemplates.at(-2)?.left
        ) *
          -props.scalingCoefficient +
        'px + (100vw - ' +
        (sequenceTemplates.at(-1)?.size * props.scalingCoefficient +
          sequenceTemplates.at(-2)?.size * props.scalingCoefficient) +
        'px)/2)',
      top:
        Math.min(sequenceTemplates.at(-1)?.top, sequenceTemplates.at(-2)?.top) *
          -props.scalingCoefficient +
        'px'
    }"
  >
    <TransitionGroup>
      <ArticleItem
        v-for="article in displayedArticles"
        :url="article.url"
        :key="article.id"
        :size="article.size * props.scalingCoefficient"
        :title="article.title"
        :image="article.image"
        :top="article.top * props.scalingCoefficient"
        :left="article.left * props.scalingCoefficient"
      />
    </TransitionGroup>
  </div>
  <div
    class="arrow-container"
    :style="{
      top:
        Math.min(sequenceTemplates.at(-1)?.top, sequenceTemplates.at(-2)?.top) +
        (sequenceTemplates.at(-1)?.size * props.scalingCoefficient) / 2 +
        'px'
    }"
  >
    <ArrowButton
      :style="{ left: '50px' }"
      direction="left"
      @click="cycleArticle(true)"
    />
    <ArrowButton
      :style="{ right: '50px' }"
      direction="right"
      @click="cycleArticle()"
    />
  </div>
</template>

<style scoped>
.article-container {
  position: relative;
  width: 0;
  height: 0;
}

.arrow-container {
  position: relative;
}

.v-enter-active {
  transition: transform 0.5s ease;
}

.v-leave-active {
  transition: none;
}

.v-enter-from,
.v-leave-to {
  transform: scale(0);
}
</style>
