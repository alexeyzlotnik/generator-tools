<script lang="ts" setup>
useHead({
  titleTemplate: title => title ? `${title} - Test title` : 'Test title',
})

</script>

<template>
  <div>
    <NuxtRouteAnnouncer />

    <NuxtLayout>
      <NuxtPage />
    </NuxtLayout>

    <UNotifications />
  </div>
</template>
