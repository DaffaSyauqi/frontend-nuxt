<template>
  <div class="w-full max-w-md mx-auto relative">
    <h1 class="text-3xl sm:text-4xl text-highlighted font-bold mb-6">
      Makanan apa yang ingin anda
      <span class="text-primary">prediksi?</span>
    </h1>

    <UInput
      v-model="model"
      placeholder="Masukkan makanan..."
      size="xl"
      class="w-full"
      @keyup.enter="submit"
    >
      <template #trailing>
        <UButton
          color="primary"
          variant="solid"
          size="xs"
          class="rounded-md"
          @click="submit"
        >
          <UIcon name="i-lucide-arrow-up" class="w-5 h-5" />
        </UButton>
      </template>
    </UInput>
  </div>
</template>

<script setup>
const props = defineProps({
  modelValue: String,
});

const emits = defineEmits(["update:modelValue", "submit"]);

const model = computed({
  get: () => props.modelValue,
  set: (v) => emits("update:modelValue", v),
});

function submit() {
  emits("submit", model.value);
}
</script>
