<script setup>
const props = defineProps(['modelValue', 'id', 'name', 'min', 'max'])
defineEmits(['update:modelValue'])
</script>

<template>
  <div class="range-slider-container">
    <label :for="props.id"
      >{{ props.name }} : <strong>{{ modelValue }}</strong></label
    >
    <div>
      <input
        type="range"
        :id="props.id"
        :min="props.min"
        :max="props.max"
        :value="modelValue"
        @input="$emit('update:modelValue', $event.target.value)"
      />
    </div>
  </div>
</template>

<style scoped>
input[type='range'] {
  width: 250px;
  accent-color: var(--color-mint);
}
</style>
