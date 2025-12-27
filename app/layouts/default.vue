<script setup lang="ts">
import type { NavigationMenuItem } from '@nuxt/ui'

const route = useRoute()
const toast = useToast()

const open = ref(false)

const links = computed(() => {
  const onSelect = () => {
    open.value = false
  }

  return [[{
    label: 'Dashboard',
    icon: 'i-lucide-house',
    to: '#dashboard'
  }, {
    label: 'Chat',
    icon: 'i-lucide-message-circle',
    to: '#chat'
  }, {
    label: 'Quotations',
    icon: 'i-lucide-file-text',
    to: '#quotations'
  }, {
    label: 'Clients',
    icon: 'i-lucide-users',
    to: '/'
  }, {
    label: 'Settings',
    icon: 'i-lucide-settings',
    to: '#settings'
  }].map(item => ({ ...item, onSelect })), [{
    label: 'Logout',
    icon: 'i-lucide-log-out',
    to: '#logout',
    class: 'bg-red-100 text-red-500 rounded-lg',
    onSelect
  }]] satisfies NavigationMenuItem[][]
})

const groups = computed(() => [{
  id: 'links',
  label: 'Go to',
  items: links.value.flat()
}, {
  id: 'code',
  label: 'Code',
  items: [{
    id: 'source',
    label: 'View page source',
    icon: 'i-simple-icons-github',
    to: `https://github.com/nuxt-ui-templates/dashboard/blob/main/app/pages${route.path === '/' ? '/index' : route.path}.vue`,
    target: '_blank'
  }]
}])

onMounted(async () => {
  const cookie = useCookie('cookie-consent')
  if (cookie.value === 'accepted') {
    return
  }

  toast.add({
    title: 'We use first-party cookies to enhance your experience on our website.',
    duration: 0,
    close: false,
    actions: [{
      label: 'Accept',
      color: 'neutral',
      variant: 'outline',
      onClick: () => {
        cookie.value = 'accepted'
      }
    }, {
      label: 'Opt out',
      color: 'neutral',
      variant: 'ghost'
    }]
  })
})
</script>

<template>
  <UDashboardGroup unit="rem">
    <UDashboardSidebar
      id="default"
      v-model:open="open"
      collapsible
      resizable
      class="bg-elevated/25"
      :ui="{ footer: 'lg:border-t lg:border-default' }"
    >
      <template #header="{ collapsed }">
        <AppLogo />
      </template>

      <template #default="{ collapsed }">
        <UNavigationMenu
          :collapsed="collapsed"
          :items="links[0]"
          orientation="vertical"
          tooltip
          popover
          :ui="{ item: 'py-2 my-2',linkLabel:'py-2' }"
        />

        <UNavigationMenu
          :collapsed="collapsed"
          :items="links[1]"
          orientation="vertical"
          tooltip
          class="mt-auto"
        />
      </template>

    </UDashboardSidebar>

    <div class="w-full flex flex-col overflow-auto">

      <UHeader :ui="{container: 'max-w-full'}" class="py-4">
        <template #left>
          <UDashboardSearchButton class="bg-transparent ring-default h-10 w-full" />
        </template>
        <template #right>
          <div class="flex items-center gap-2">
            <NotificationBtn />
            <LanguageSelector />
            <UserMenu />
            <ColorModeButton />
          </div>
        </template>
    </UHeader>
    <main class="w-full flex flex-col px-8 overflow-auto">
      <slot />
    </main>
    <UDashboardSearch :groups="groups" />
  </div>
  </UDashboardGroup>
</template>
