<script setup lang="ts">
const items = [{
  label: 'English',
  avatar: {
    src: 'https://flagcdn.com/w40/us.png',
    alt: 'US Flag'
  },
  click: () => console.log('English')
}, {
  label: 'Arabic',
  avatar: {
    src: 'https://flagcdn.com/w40/sa.png',
    alt: 'SA Flag'
  },
  click: () => console.log('Arabic')
}]
</script>

<template>
  <UDropdownMenu :items="[items]" :content="{ align: 'end' }">
    <UButton
      color="neutral"
      variant="ghost"
      label="English"
      trailing-icon="i-lucide-chevron-down"
      class="w-full"
    >
      <template #leading>
        <img src="https://flagcdn.com/w40/us.png" alt="US Flag" class="w-5 h-auto rounded-xs shrink-0" />
      </template>
    </UButton>
  </UDropdownMenu>
</template>
