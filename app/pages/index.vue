<script setup lang="ts">
import { ref, computed, h, watch } from 'vue'
import { getPaginationRowModel } from '@tanstack/vue-table'
import type { TableColumn } from '@nuxt/ui'

const table = useTemplateRef('table')

// Mock data for clients to match the example image
type Client = {
  id: number
  name: string
  image: string
  phone: string
  membership: 'active' | 'expired'
}

const data = ref<Client[]>([
  {
    id: 1,
    name: 'Hosam Halim',
    image: 'https://randomuser.me/api/portraits/men/32.jpg',
    phone: '+966509021089',
    membership: 'active'
  },
  {
    id: 2,
    name: 'Kathryn Murphy',
    image: 'https://randomuser.me/api/portraits/women/44.jpg',
    phone: '+966539448021',
    membership: 'expired'
  },
  {
    id: 3,
    name: 'Ralph Edwards',
    image: 'https://randomuser.me/api/portraits/men/52.jpg',
    phone: '+966503509238',
    membership: 'active'
  },
  {
    id: 4,
    name: 'Robert Fox',
    image: 'https://randomuser.me/api/portraits/men/58.jpg',
    phone: '+966505091629',
    membership: 'active'
  },
  {
    id: 5,
    name: 'Albert Flores',
    image: 'https://randomuser.me/api/portraits/men/36.jpg',
    phone: '+966509435702',
    membership: 'active'
  },
  {
    id: 6,
    name: 'Jacob Jones',
    image: 'https://randomuser.me/api/portraits/men/18.jpg',
    phone: '+966539474857',
    membership: 'active'
  },
  {
    id: 7,
    name: 'Cody Fisher',
    image: 'https://randomuser.me/api/portraits/men/71.jpg',
    phone: '+966503809306',
    membership: 'active'
  },
  {
    id: 8,
    name: 'Ralph Edwards',
    image: 'https://randomuser.me/api/portraits/men/33.jpg',
    phone: '+966505091629',
    membership: 'active'
  },
  {
    id: 9,
    name: 'Robert Fox',
    image: 'https://randomuser.me/api/portraits/men/58.jpg',
    phone: '+966509435702',
    membership: 'active'
  },
  {
    id: 10,
    name: 'Jacob Jones',
    image: 'https://randomuser.me/api/portraits/men/18.jpg',
    phone: '+966539474857',
    membership: 'active'
  }
])

const selection = ref<number[]>([])

const active = ref('0')

const columns: TableColumn<Client>[] = [
  {
    id: 'checkbox',
    header: () => h('input', { 'type': 'checkbox', 'class': 'accent-primary', 'aria-label': 'Select all', 'style': 'cursor:pointer;' }),
    cell: ({ row }: any) =>
      h('input', {
        type: 'checkbox',
        class: 'accent-primary',
        checked: selection.value.includes(row.original.id),
        onClick: (e: Event) => {
          const checked = (e.target as HTMLInputElement).checked
          if (checked && !selection.value.includes(row.original.id)) {
            selection.value.push(row.original.id)
          } else if (!checked) {
            selection.value.splice(selection.value.indexOf(row.original.id), 1)
          }
        }
      }),
    width: 40 as any
  },
  {
    accessorKey: 'image',
    id: 'image',
    header: 'Image',
    cell: ({ row }: any) =>
      h('img', {
        src: row.getValue('image'),
        alt: row.original.name,
        class: 'h-8 w-8 rounded-full object-cover border'
      }),
    width: 60 as any
  },
  {
    accessorKey: 'name',
    id: 'name',
    header: () => h('span', { class: 'font-medium' }, 'Client Name'),
    cell: ({ row }: any) => h('span', { class: 'font-semibold text-gray-900 dark:text-white' }, row.original.name)
  },
  {
    accessorKey: 'phone',
    id: 'phone',
    header: 'Phone'
  },
  {
    accessorKey: 'membership',
    id: 'membership',
    header: () => h('span', { class: 'font-medium' }, 'Membership Status'),
    cell: ({ row }: any) =>
      h(
        'span',
        {
          class:
            row.getValue('membership') === 'active'
              ? 'bg-green-100 text-green-700 px-3 py-1 rounded-full text-xs font-medium'
              : 'bg-red-100 text-red-700 px-3 py-1 rounded-full text-xs font-medium'
        },
        row.getValue('membership') === 'active' ? 'Active' : 'Expired'
      )
  },
  {
    id: 'actions',
    header: 'Actions',
    cell: () =>
      h(
        'div',
        { class: 'flex gap-2 justify-center' },
        [
          h('button', { 'class': 'text-gray-500 hover:text-primary', 'aria-label': 'Chat' },
            [h('i', { class: 'i-heroicons-chat-bubble-left-ellipsis-20-solid text-lg' })]
          )
        ]
      )
  }
]

const pagination = ref({
  pageIndex: 0,
  pageSize: 20
})

const globalFilter = ref('')

const filteredData = computed(() => {
  const duplicated = Array(3).fill(data.value).flat()
  return duplicated.filter(
    client =>
      client.name.toLowerCase().includes(globalFilter.value.toLowerCase())
      || client.phone.includes(globalFilter.value)
  )
})

// Compute paged data to show only items for the current page according to pageSize
const pagedData = computed(() => {
  const start = pagination.value.pageIndex * pagination.value.pageSize
  const end = start + pagination.value.pageSize
  return filteredData.value.slice(start, end)
})

const tabs = [
  {
    label: 'List',
    icon: 'i-lucide:list',
    slot: 'list'
  },
  {
    label: 'Grid',
    icon: 'i-lucide:layout-grid',
    slot: 'grid'
  }
]

// Watch for pageSize changes and reset pageIndex to 0 if needed (so user doesn't land on empty page)
watch(() => pagination.value.pageSize, () => {
  pagination.value.pageIndex = 0
})
</script>

<template>
  <div class="w-full mx-auto flex flex-col gap-8 px-5 py-6 my-6">
    <UPageHeader title="Clients" />
    <div class="w-full mx-auto rounded-xl flex flex-col gap-8 border border-gray-100 shadow-sm px-5 py-6 my-6">
      <!-- <h2 class="font-semibold text-[16px] leading-none text-gray-700 mb-1 sm:mb-0">
        Clients
      </h2> -->
      <div class="flex flex-col gap-2 sm:flex-row sm:items-center sm:justify-between mb-2">
        <div class="flex gap-2 items-center">
          <UInput
            v-model="globalFilter"
            :placeholder="'Search...'"
            icon="i-lucide:search"
            class="h-9 w-[250px]"
          />
          <UButton icon="i-lucide:circle-plus" label="Client Name" color="neutral" variant="outline" />
          <UButton icon="i-lucide:circle-plus" label="Phone" color="neutral" variant="outline" />
          <UButton icon="i-lucide:circle-plus" label="Membership" color="neutral" variant="outline" />
        </div>
        <UTabs
          v-model="active"
          :content="false"
          :items="tabs"
          class="w-fit"
          color="neutral"
        />
      </div>
      <UTabs v-model="active" :ui="{ list: 'hidden' }" :items="tabs">
        <template #list>
          <div class="rounded-xl border border-default shadow-sm overflow-auto">
            <UTable
              ref="table"
              v-model:pagination="pagination"
              :data="pagedData"
              :columns="columns"
              :pagination-options="{ getPaginationRowModel: getPaginationRowModel() }"
              :class="'min-w-[900px]'"
              :thead-class="'bg-gray-50 text-gray-500 text-xs font-semibold border-b border-gray-100'"
              :tbody-class="'divide-y divide-gray-50'"
              row-class="hover:bg-gray-50 transition"
              :row-key="(row: Client) => row.id"
            />
          </div>
        </template>
        <template #grid>
          <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-6">
            <template v-for="client in pagedData" :key="client.id">
              <div
                class="flex flex-col items-center bg-white dark:bg-gray-900 rounded-xl border border-gray-100 dark:border-gray-800 shadow-sm p-6 relative transition hover:shadow-md"
              >
                <UAvatar
                  :src="client.image"
                  :alt="client.name"
                  size="xl"
                  class="mb-4"
                />
                <UBadge
                  :color="client.membership === 'active' ? 'success' : 'error'"
                  variant="soft"
                  size="lg"
                  class="absolute left-4 top-4"
                >
                  {{ client.membership.charAt(0).toUpperCase() + client.membership.slice(1) }}
                </UBadge>
                <div class="text-center mt-2">
                  <div class="font-semibold text-gray-900 dark:text-white text-base">
                    {{ client.name }}
                  </div>
                  <div class="text-gray-500 dark:text-gray-400 text-sm mt-1">
                    {{ client.phone }}
                  </div>
                </div>
                <div class="flex gap-2 mt-5 w-full">
                  <UButton
                    color="primary"
                    label="View Profile"
                    size="sm"
                    block
                    class="flex-1 dark:bg-white"
                  />
                  <UButton
                    color="neutral"
                    variant="soft"
                    label="Message"
                    size="sm"
                    block
                    class="flex-1"
                  />
                </div>
              </div>
            </template>
          </div>
        </template>
      </UTabs>
      <footer class="flex justify-between w-full">
        <span>
          <span v-if="selection.length">{{ selection.length }}</span>
          <span v-else>0</span>
          of {{ data.length }} rows selected
        </span>
        <div class="flex gap-4 items-center">
          <label class="text-xs text-gray-400">
            Rows per page
            <USelect
              v-model="pagination.pageSize"
              :items="[10, 20, 50, 100]"
              size="xs"
              class="ml-2 w-16"
            />
          </label>
          <UPagination
            :page="pagination.pageIndex + 1"
            :items-per-page="pagination.pageSize"
            :total="filteredData.length"
            @update:page="(p) => { pagination.value.pageIndex = p - 1 }"
          />
          <span class="text-xs text-gray-400">
            Page {{ pagination.pageIndex + 1 }} of {{ Math.ceil(filteredData.length / pagination.pageSize) || 1 }}
          </span>
        </div>
      </footer>
    </div>
  </div>
</template>
