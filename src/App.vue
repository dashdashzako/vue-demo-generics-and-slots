<script setup lang="ts">
import ActivityList from './components/ActivityList.vue';
import type { BaseActivityItem } from './types'

type MyNumberItem = BaseActivityItem & {
  stringValue: string
}

const numberItems: MyNumberItem[] = [
  {
    id: 1,
    stringValue: 'one'
  },
  {
    id: 2,
    stringValue: 'two'
  }
]

type MixedItem = BaseActivityItem & {
  theValue: number | string
}

const mixedItems: MixedItem[] = [
  {
    id: 1,
    theValue: 'this is a string'
  },
  {
    id: 2,
    theValue: 12345
  }
]

</script>

<template>
  <ActivityList title="Some Number Items" :items="numberItems" v-slot="{ item }">
    {{ item.stringValue }}
  </ActivityList>


  <ActivityList title="Some Mixed Items" :items="mixedItems" v-slot="{ item }">
    <code v-if="typeof item.theValue === 'number'">{{ item.theValue }}</code>
    <p v-else-if="typeof item.theValue === 'string'">{{ item.theValue }}</p>
    <div v-else>?????</div>
  </ActivityList>
</template>
