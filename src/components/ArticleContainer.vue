<script setup>
import ArticleItem from './ArticleItem.vue'
import { computed, ref, onMounted } from 'vue'

const props = defineProps(['articleList', 'nbElements', 'scalingCoefficient'])

const currentArticle = ref(0)
const sequenceTemplates = ref([])

onMounted(() => {
  console.log(props.nbElements)
  currentArticle.value = props.nbElements
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
    <div class="arrow left" @click="cycleArticle(true)"></div>
    <div class="arrow right" @click="cycleArticle()"></div>
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

.arrow {
  position: absolute;
  width: 0px;
  height: 0px;
  border-style: solid;
  border-width: 0 33px 50px 33px;
  border-color: transparent transparent #34495e transparent;
}

.arrow:hover {
  cursor: pointer;
  border-color: transparent transparent #41b883 transparent;
}

.arrow.left {
  transform: rotate(270deg);
  left: 50px;
}

.arrow.right {
  transform: rotate(90deg);
  right: 50px;
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
