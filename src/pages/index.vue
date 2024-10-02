<script setup lang="ts">
import { useVirtualList, useInfiniteScroll } from '@vueuse/core'

const data = ref(Array.from(Array(50).keys(), () => 'Hello Scroll'))

const { list, containerProps, wrapperProps } = useVirtualList(data, {
  // Keep `itemHeight` in sync with the item's row.
  itemHeight: 40
})

useInfiniteScroll(
  containerProps.ref,
  () => {
    data.value.push(...Array.from(Array(10).keys(), () => 'Hello Scroll'))
  },
  {
    distance: 0
  }
)
</script>

<template>
  <div>
    <div v-bind="containerProps" class="mt-20 h-80 rounded border p-2">
      <div v-bind="wrapperProps" class="mx-auto max-w-sm">
        <div
          v-for="item in list"
          :key="item.index"
          class="flex flex-col justify-center border px-4 py-2"
        >
          Row: {{ `${item.index} - ${item.data}` }}
        </div>
      </div>
    </div>
    <p class="pt-10 text-right">Loaded Items: {{ data.length }}</p>
  </div>
</template>
