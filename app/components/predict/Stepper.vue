<template>
  <UStepper
    v-model="model"
    :items="items"
    class="mt-auto pointer-events-none"
    :clickable="false"
  />
</template>

<script setup>
const props = defineProps({
  modelValue: Number,
  items: Array,
});

const emit = defineEmits(["update:modelValue"]);

const model = computed({
  get: () => props.modelValue,
  set: (v) => emit("update:modelValue", v),
});
</script>
