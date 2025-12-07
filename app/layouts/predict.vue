<template>
  <div class="flex p-2">
    <NuxtLink to="/">
      <UButton
        icon="i-lucide-chevron-left"
        color="neutral"
        size="md"
        variant="ghost"
      >
        Kembali
      </UButton>
    </NuxtLink>

    <div class="ml-auto">
      <UColorModeButton />
    </div>
  </div>

  <slot></slot>
</template>

<script></script>
