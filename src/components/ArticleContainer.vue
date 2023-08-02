<script setup>
import ArticleItem from './ArticleItem.vue'
import { computed, ref, onMounted } from 'vue'

const props = defineProps(['articleList', 'nbElements', 'scalingCoefficient'])

const currentArticle = ref(0)
const sequenceTemplates = ref([])

const randomHsl = () => `hsla(${Math.random() * 360}, 100%, 50%, 1)`

const transformFromTemplate = (
  source,
  template,
  isNew = false,
  index = null
) => {
  source.textContent = template.size
  source.style.fontSize = (template.size * props.scalingCoefficient) / 3 + 'px'
  source.style.width = source.style.height =
    template.size * props.scalingCoefficient + 'px'
  source.style.top = template.top + 'px'
  source.style.left = template.left + 'px'
  if (isNew) {
    source.classList.add('box-item')
    source.style.backgroundColor = randomHsl()
  }
  return true
}

onMounted(() => {
  sequenceTemplates.value[0] = {}
  sequenceTemplates.value[0].size =
    sequenceTemplates.value[0].fontSize =
    sequenceTemplates.value[0].top =
    sequenceTemplates.value[0].left =
      0

  sequenceTemplates.value[1] = {}
  sequenceTemplates.value[1].size = 1
  sequenceTemplates.value[1].fontSize = 1 / 3
  sequenceTemplates.value[1].top = sequenceTemplates.value[1].left = 0

  for (let i = 2; i < props.nbElements; i++) {
    let itemSize =
      sequenceTemplates.value[i - 1].size + sequenceTemplates.value[i - 2].size
    let previousElement = sequenceTemplates.value[i - 1]

    sequenceTemplates.value[i] = {}
    sequenceTemplates.value[i].size = itemSize
    sequenceTemplates.value[i].fontSize = itemSize / 3
    if (i % 4 === 0) {
      // bottom left anchor
      sequenceTemplates.value[i].top =
        previousElement.top + previousElement.size
      sequenceTemplates.value[i].left = previousElement.left
    } else if (i % 4 === 1) {
      // bottom right anchor
      sequenceTemplates.value[i].top =
        previousElement.top + previousElement.size - itemSize
      sequenceTemplates.value[i].left =
        previousElement.left + previousElement.size
    } else if (i % 4 === 2) {
      // top right anchor
      sequenceTemplates.value[i].top = previousElement.top - itemSize
      sequenceTemplates.value[i].left =
        previousElement.left + previousElement.size - itemSize
    } else if (i % 4 === 3) {
      // top left anchor
      sequenceTemplates.value[i].top = previousElement.top
      sequenceTemplates.value[i].left = previousElement.left - itemSize
    }
  }
})

const displayedArticles = computed(() => {
  return sequenceTemplates.value.map((element, index) => {
    return {
      ...element,
      title: props.articleList[currentArticle.value + index]?.title,
      image: props.articleList[currentArticle.value + index]?.urlToImage,
      id: props.articleList[currentArticle.value + index]?.id
    }
  })
})

const cycleArticle = () => {
  let next = --currentArticle.value
  if (next < 0) next = props.articleList.length - props.nbElements
  // currentArticle.value = (currentArticle.value - 1) % props.articleList.length
  currentArticle.value = next
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
    <ArticleItem
      v-for="article in displayedArticles"
      :key="article.id"
      :size="article.size * props.scalingCoefficient"
      :title="article.title"
      :image="article.image"
      :top="article.top * props.scalingCoefficient"
      :left="article.left * props.scalingCoefficient"
      @click="cycleArticle"
    />
  </div>
</template>

<style scoped>
.article-container {
  position: relative;
  width: 0;
  height: 0;
}
</style>
