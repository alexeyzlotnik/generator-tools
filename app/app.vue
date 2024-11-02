<script lang="ts" setup>
useHead({
  titleTemplate: title => title ? `${title} – Free online tools for developers and marketers` : 'Tool Farm – Free online tools for developers and marketers',
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
