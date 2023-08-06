<script setup>
import { computed } from 'vue'
const props = defineProps({
  direction: {
    type: String,
    required: true,
    validator(value) {
      return ['left', 'right'].includes(value)
    }
  }
})
const directionClass = computed(() => {
  return { [props.direction]: true }
})
</script>
<template>
  <div :class="['arrow', directionClass]"></div>
</template>

<style scoped>
.arrow {
  position: absolute;
  width: 60px;
  height: 60px;
  border-radius: 50%;
  background-color: #ffffff33;
}

.arrow::after {
  width: 100%;
  height: 100%;
  position: absolute;
  text-align: center;
  font-weight: bold;
  font-size: 40px;
  line-height: 56px;
  content: '▶';
  color: var(--color-mint);
}

.arrow:hover {
  cursor: pointer;
}

.arrow:hover::after {
  color: #eee;
}

.arrow.left::after {
  transform: scaleX(-1);
  left: -3px;
}

.arrow.right:after {
  right: -3px;
}
</style>
