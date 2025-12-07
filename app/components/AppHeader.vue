<script setup lang="ts">
const route = useRoute();

const items = computed(() => [
  {
    label: "Prediksi!",
    to: "/predict",
  },
  {
    label: "Fitur",
    to: "#features",
  },
  {
    label: "FAQ",
    to: "#faq",
  },
]);
</script>

<template>
  <UHeader>
    <template #left>
      <NuxtLink to="/" class="flex items-center">
        <AppLogo class="w-10 h-10" />
        <h1 class="ml-2 text-xl font-semibold">
          UMKM-<span class="text-primary">Tool</span>
        </h1>
      </NuxtLink>
    </template>

    <UNavigationMenu :items="items" variant="link" />

    <template #right>
      <UColorModeButton />
    </template>

    <template #body>
      <UNavigationMenu :items="items" orientation="vertical" class="-mx-2.5" />
    </template>
  </UHeader>
</template>
