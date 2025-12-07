<script setup lang="ts">
const { data: page } = await useAsyncData("landing", () =>
  queryCollection("index").first(),
);

const title = page.value?.seo?.title || page.value?.title;
const description = page.value?.seo?.description || page.value?.description;

useSeoMeta({
  titleTemplate: "",
  title,
  ogTitle: title,
  description,
  ogDescription: description,
});
</script>

<template>
  <div v-if="page">
    <UPageHero
      :title="page.title"
      :description="page.description"
      :links="page.hero?.links"
    >
      <template #top>
        <HeroBackground />
      </template>

      <template #title>
        <MDC :value="page.title" unwrap="p" />
      </template>

      <!-- <PromotionalVideo /> -->
    </UPageHero>

    <section id="features">
      <UPageSection
        :title="page.features.title"
        :description="page.features.description"
      >
        <UPageGrid>
          <UPageCard
            v-for="(item, index) in page.features.items"
            :key="index"
            v-bind="item"
            spotlight
          />
        </UPageGrid>
      </UPageSection>
    </section>

    <section id="faq">
      <UPageSection :title="page.faq.title" :description="page.faq.description">
        <UAccordion
          :items="page.faq.items"
          :unmount-on-hide="false"
          type="multiple"
          class="max-w-3xl mx-auto"
          :ui="{
            trigger: 'text-base text-highlighted',
            body: 'text-base text-muted',
          }"
        />
      </UPageSection>
    </section>

    <USeparator />

    <UPageCTA v-bind="page.cta" variant="naked" class="overflow-hidden">
      <LazyStarsBg />
    </UPageCTA>
  </div>
</template>
