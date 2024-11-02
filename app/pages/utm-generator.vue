<template>
  <UContainer>
    <UPage>
      <UPageHeader
        title="UTM Generator - Create & Track Marketing Campaign URLs"
        description="Free UTM parameter generator to create trackable URLs for your marketing campaigns. Build custom UTM tags for Google Analytics to measure traffic sources, campaigns, and content performance."
        :ui="{title: 'md:text-center w-full', description: 'md:text-center w-full', wrapper: 'border-0'}"
      />
      <UPageBody class="flex flex-col gap-12">
        <UTMGenerator/>
        <UDivider label="Learn more about UTM generation" />
        <UTMGeneratorSEO />
      </UPageBody>
    </UPage>
  </UContainer>
</template>

<script setup>
  useSeoMeta({
    title: 'UTM Generator - Create & Track Marketing Campaign URLs',
    ogTitle: 'UTM Generator - Create & Track Marketing Campaign URLs',
    description: 'Free UTM parameter generator to create trackable URLs for your marketing campaigns. Build custom UTM tags for Google Analytics to measure traffic sources, campaigns, and content performance.',
    ogDescription: 'Free UTM parameter generator to create trackable URLs for your marketing campaigns. Build custom UTM tags for Google Analytics to measure traffic sources, campaigns, and content performance.',
  })
</script>

<style>

</style>