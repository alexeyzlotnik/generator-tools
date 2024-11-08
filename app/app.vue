<script lang="ts" setup>
useHead({
  titleTemplate: title => title ? `${title} – Free online tools for developers and marketers` : 'Tool Farm – Free online tools for developers and marketers',
  script: [
    // Add the Google Analytics script
    { src: "https://www.googletagmanager.com/gtag/js?id=G-TVCD376VGN", async: true },
    {
      innerHTML: `
        window.dataLayer = window.dataLayer || [];
        function gtag(){dataLayer.push(arguments);}
        gtag('js', new Date());
        gtag('config', 'G-TVCD376VGN');
      `,
      type: 'text/javascript'
    }
  ]
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
