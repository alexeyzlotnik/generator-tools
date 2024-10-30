<template>
  <UContainer>
    <UPage>
      <UPageHeader
        title="title"
        description="description"
        :ui="{title: 'md:text-center w-full', description: 'md:text-center w-full', wrapper: 'border-0'}"
      />
      <UPageBody class="flex flex-col gap-12">
        <WordcloudGenerator/>
        <UDivider label="Learn more about wordcloud generation" />
        <!-- <WordcloudGeneratorSEO /> -->
      </UPageBody>
    </UPage>
  </UContainer>
</template>

<script setup>
  // useSeoMeta({
  //   title: 'Secure Password Generator - Create Strong & Random Passwords Online',
  //   ogTitle: 'Secure Password Generator - Create Strong & Random Passwords Online',
  //   description: 'Generate secure, random passwords with a mix of uppercase, lowercase, numbers, and symbols. Customize password length and strength to protect your accounts from cyber threats.',
  //   ogDescription: 'Generate secure, random passwords with a mix of uppercase, lowercase, numbers, and symbols. Customize password length and strength to protect your accounts from cyber threats.',
  // })
</script>

<style>

</style>