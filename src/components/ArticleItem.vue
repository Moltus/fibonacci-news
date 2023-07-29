<script setup>
const props = defineProps(['content'])
</script>

<template>
  <div>
    <p>{{ props.content.title }}</p>
    <button @click="$parent.$emit('handleClickOnViewed', props.content.id)">
      Mark as viewed
    </button>
  </div>
</template>
