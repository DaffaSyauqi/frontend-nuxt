<template>
  <div class="max-w-2xl w-full text-center">
    <h2 class="text-3xl font-bold mb-6">Hasil Perhitungan</h2>

    <div v-if="errorMsg" class="text-left bg-neutral p-6 rounded-xl">
      <p class="text-red-500">{{ errorMsg }}</p>
    </div>

    <div
      v-else-if="result"
      class="text-left bg-neutral border border-neutral/20 p-6 rounded-xl"
    >
      <p><strong>Nama Produk:</strong> {{ result.product_name }}</p>

      <p>
        <strong>Estimasi Output:</strong>
        {{ result.estimated_output }} {{ result.output_unit }}
      </p>

      <h3 class="text-xl font-bold mt-4">Detail</h3>
      <ul class="list-disc ml-6">
        <li>
          <strong>Minimal:</strong>
          {{ result.details.low_estimate }} {{ result.output_unit }}
        </li>
        <li>
          <strong>Maksimal:</strong>
          {{ result.details.high_estimate }} {{ result.output_unit }}
        </li>
        <li>
          <strong>Faktor Pembatas:</strong>
          {{ result.details.limiting_factor }}
        </li>
        <li>
          <strong>Kalkulasi:</strong>
          {{ result.details.calculation_reason }}
        </li>
      </ul>

      <h3 class="text-xl font-bold mt-4">Alternatif</h3>
      <ul class="list-disc ml-6">
        <li v-for="(alt, i) in result.alternatives" :key="i">
          {{ alt.name }} — {{ alt.estimated_output }} {{ alt.unit }}
        </li>
      </ul>

      <div class="flex w-full mt-6 text-center justify-end">
        <UButton :loading="loading" @click="$emit('reset')">Coba Lagi</UButton>
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps({
  loading: Boolean,
  errorMsg: String,
  result: Object,
  loading: Boolean,
});

defineEmits(["reset"]);
</script>
