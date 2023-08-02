<script setup>
const props = defineProps(['size', 'title', 'image', 'left', 'top', 'url'])
</script>

<template>
  <a
    class="article-item"
    :href="props.url"
    target="_blank"
    :style="{
      background:
        // 'center / contain no-repeat linear-gradient(rgba(0, 0, 0, 0.9), rgba(0, 0, 0, 0.0)), url(' +
        'center/cover no-repeat url(' + props.image + ')',
      width: props.size + 'px',
      height: props.size + 'px',
      top: props.top + 'px',
      left: props.left + 'px',
      fontSize: props.size / 40 + 10 + 'px'
    }"
  >
    <p v-if="props.size > 200">{{ props.title }}</p>
  </a>
</template>

<style scoped>
.article-item {
  transition: all 0.5s ease;
  color: #ccc;
  text-shadow: 0 0 5px var(--color-background);
  position: absolute;
  user-select: none;
  box-sizing: border-box;
  border: solid 5px var(--color-background);
}

.article-item p {
  padding: 0.5rem 1rem 1rem 1rem;
  background-color: #00000060;
}
</style>
