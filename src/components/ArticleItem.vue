<script setup>
const props = defineProps(['content', 'index'])
</script>

<template>
  <div>
    <p>{{ props.content.title }}</p>
    <button @click="$parent.$emit('handleClickOnViewed', props.index)">
      Mark as viewed
    </button>
  </div>
</template>
