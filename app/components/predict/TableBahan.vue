<template>
  <div class="max-w-2xl w-full">
    <h2 class="text-2xl font-bold mb-4">
      Masukkan semua bahan yang kamu punya:
    </h2>

    <UTable :data="data" :columns="columns" />

    <div class="flex justify-between mt-6 gap-4">
      <UButton :loading="loading" icon="i-lucide-plus" @click="emit('add')">
        Tambah Bahan Baru
      </UButton>

      <UButton :loading="loading" :disabled="loading" @click="emit('next')">
        Hitung Porsi
      </UButton>
    </div>
  </div>
</template>

<script setup>
import { h, resolveComponent } from "vue";

const UInput = resolveComponent("UInput");
const USelect = resolveComponent("USelect");
const UButton = resolveComponent("UButton");

const props = defineProps({
  data: Array,
  satuanItems: Array,
  loading: Boolean,
});

const emit = defineEmits(["add", "next", "remove-row"]);

const columns = [
  {
    accessorKey: "name",
    header: "Bahan",
    cell: ({ row }) =>
      h(UInput, {
        modelValue: row.original.name,
        placeholder: "Nama bahan...",
        class: "w-full",
        "onUpdate:modelValue": (val) => (row.original.name = val),
      }),
  },
  {
    accessorKey: "amount",
    header: "Jumlah",
    cell: ({ row }) =>
      h(UInput, {
        type: "number",
        modelValue: row.original.amount,
        placeholder: "Jumlah...",
        class: "w-full",
        "onUpdate:modelValue": (val) => (row.original.amount = Number(val)),
      }),
  },
  {
    accessorKey: "unit",
    header: "Satuan",
    cell: ({ row }) =>
      h(USelect, {
        modelValue: row.original.unit,
        items: props.satuanItems,
        class: "w-full",
        "onUpdate:modelValue": (val) => (row.original.unit = val),
      }),
  },
  {
    header: "Hapus",
    cell: ({ row }) =>
      h(
        UButton,
        {
          variant: "ghost",
          size: "xs",
          icon: "i-lucide-x",
          color: "neutral",
          onClick: () => emit("remove-row", row.index),
        },
        () => "Hapus",
      ),
  },
];
</script>
