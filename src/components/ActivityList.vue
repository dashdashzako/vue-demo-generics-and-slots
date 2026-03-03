<script setup lang="ts" generic="T extends BaseActivityItem">
import type { BaseActivityItem } from '../types'

type Props = {
  title: string
  items: T[]
}

const props = defineProps<Props>()

defineSlots<{
  default(props: { item: T }): unknown
}>()
</script>

<template>
  <section>
    <h2>{{ props.title }}</h2>
    <ol>
      <li v-for="item in props.items" :key="item.id">
        <slot :item />
      </li>
    </ol>
  </section>
</template>
