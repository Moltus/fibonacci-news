<script setup>
const props = defineProps(['size', 'title', 'image', 'left', 'top'])
console.log(props)
</script>

<template>
  <div
    class="article-item"
    :style="{
      background:
        'center / contain no-repeat linear-gradient(rgba(0, 0, 0, 0.9), rgba(0, 0, 0, 0.0)), url(' +
        props.image +
        ')',
      width: props.size + 'px',
      height: props.size + 'px',
      top: props.top + 'px',
      left: props.left + 'px',
      fontSize: props.size / 20 + 'px'
    }"
  >
    <p>{{ props.title }}</p>
  </div>
</template>

<style scoped>
.article-item {
  position: absolute;
  user-select: none;
  box-sizing: border-box;
  border: solid 1px var(--color-background);
}

.article-item p {
  padding: 0.5rem;
  background-color: #00000050;
}
</style>
