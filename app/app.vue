<script setup lang="ts">
const colorMode = useColorMode()

const color = computed(() => colorMode.value === 'dark' ? '#1b1718' : 'white')

useHead({
  meta: [
    { charset: 'utf-8' },
    { name: 'viewport', content: 'width=device-width, initial-scale=1' },
    { key: 'theme-color', name: 'theme-color', content: color }
  ],
  link: [
    { rel: 'icon', type: 'image/png', sizes: '32x32', href: '/favicon.ico' },
    { rel: 'icon', type: 'image/png', sizes: '192x192', href: '/web-app-manifest-192x192.png' },
    { rel: 'icon', type: 'image/png', sizes: '512x512', href: '/web-app-manifest-512x512.png' },
    { rel: 'apple-touch-icon', sizes: '180x180', href: '/apple-icon.png' },
    { rel: 'icon', type: 'image/svg+xml', href: '/icon0.svg' },
    { rel: 'icon', type: 'image/png', href: '/icon1.png' },
    { rel: 'icon', type: 'image/png', href: '/logo.png' }
  ],
  htmlAttrs: {
    lang: 'en'
  }
})

const title = 'Dashboard Wosol Task'
const description = 'Dashboard Wosol Task is a modern admin solution built with Nuxt UI, offering advanced data visualization, customizable layouts, and essential tools to streamline management tasks.'

useSeoMeta({
  title,
  description,
  ogTitle: title,
  ogDescription: description,
  // ogImage: '',
  // twitterImage: '',
  twitterCard: 'summary_large_image'
})
</script>

<template>
  <UApp>
    <NuxtLoadingIndicator />

    <NuxtLayout>
      <NuxtPage />
    </NuxtLayout>
  </UApp>
</template>
