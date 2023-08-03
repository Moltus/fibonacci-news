<script setup>
const props = defineProps(['modelValue', 'id', 'name', 'min', 'max'])
defineEmits(['update:modelValue'])
</script>

<template>
  <div>
    <label
      :for="props.id"
      class="mb-2 inline-block text-neutral-700 dark:text-neutral-200"
      >{{ props.name }}</label
    >
    <input
      type="range"
      class="transparent h-[4px] w-full cursor-pointer appearance-none border-transparent bg-neutral-200 dark:bg-neutral-600"
      :id="props.id"
      :min="props.min"
      :max="props.max"
      :value="modelValue"
      @input="$emit('update:modelValue', $event.target.value)"
    />
  </div>
</template>
