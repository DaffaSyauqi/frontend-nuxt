<template>
  <div class="flex flex-col min-h-screen">
    <div
      class="flex flex-col justify-center items-center flex-grow py-10 w-full"
    >
      <UContainer class="w-full flex flex-col items-center text-center">
        <PredictInputMakanan
          v-if="step === 0"
          v-model="makanan"
          @submit="goToStep2"
        />

        <PredictTableBahan
          v-if="step === 1"
          :data="data"
          :satuan-items="satuanItems"
          :loading="loading"
          @add="addRow"
          @remove-row="removeRow"
          @next="goToStep3"
        />

        <PredictHasil
          v-if="step === 2"
          :loading="loading"
          :errorMsg="errorMsg"
          :result="result"
          @reset="reset"
        />
      </UContainer>
    </div>

    <PredictStepper v-model="step" :items="items" />
  </div>
</template>

<script setup lang="ts">
definePageMeta({ layout: "predict" });

import { ref } from "vue";

const step = ref(0);
const makanan = ref("");
const loading = ref(false);
const errorMsg = ref("");
const result = ref<any>(null);

interface Bahan {
  name: string;
  amount: number | null;
  unit: string;
}

const data = ref<Bahan[]>([]);

const satuanItems = [
  { label: "buah", value: "buah" },
  { label: "gram", value: "gram" },
  { label: "ml", value: "ml" },
  { label: "kg", value: "kg" },
  { label: "liter", value: "liter" },
  { label: "sendok makan", value: "sendok makan" },
  { label: "sendok teh", value: "sendok teh" },
  { label: "cangkir", value: "cangkir" },
];

function addRow() {
  data.value.push({ name: "", amount: 0, unit: "buah" });
}
function removeRow(index: number) {
  data.value.splice(index, 1);
}

function goToStep2(value: string) {
  makanan.value = value;
  step.value = 1;
}

async function goToStep3() {
  loading.value = true;
  errorMsg.value = "";
  result.value = null;

  try {
    const body = {
      food_name: makanan.value,
      ingredients: data.value.map((i) => ({
        ...i,
        amount: Number(i.amount) || 0,
      })),
    };

    const res: any = await $fetch("http://127.0.0.1:8000/api/v1/predict", {
      method: "POST",
      body,
      headers: { "Content-Type": "application/json" },
    });

    result.value = res.result;
    step.value = 2;
  } catch (err: any) {
    errorMsg.value = err?.data?.error || "Terjadi kesalahan.";
  } finally {
    loading.value = false;
  }
}

function reset() {
  step.value = 0;
  makanan.value = "";
  data.value = [];
  result.value = null;
  errorMsg.value = "";
  loading.value = false;
}

const items = [
  { title: "Makanan", description: "Masukkan makanan" },
  { title: "Bahan", description: "Bahan yang dimiliki" },
  { title: "Hasil", description: "Perhitungan porsi" },
];
</script>
