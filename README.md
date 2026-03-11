# Vue Demo — Generics & Slots

This project demonstrates **generic components with typed slots** in Vue 3, a feature introduced in Vue 3.3.

## Purpose

The goal is to show how to build a reusable list component that:

- accepts any type of items via a TypeScript generic parameter
- exposes a slot whose type is automatically inferred from the data passed in
- guarantees type safety in the parent template without any manual casting

## How it works

### The base type

```ts
// src/types.ts
export type BaseActivityItem = {
  id: number;
};
```

All items passed to the component must at minimum have a numeric `id`.

### The generic `ActivityList` component

```vue
<!-- src/components/ActivityList.vue -->
<script setup lang="ts" generic="T extends BaseActivityItem">
type Props = {
  title: string
  items: T[]
}

defineProps<Props>()

defineSlots<{
  default(props: { item: T }): unknown
}>()
</script>
```

The `generic="T extends BaseActivityItem"` attribute on `<script setup>` declares a type parameter `T`.
`defineSlots` binds that `T` to the `default` slot, which lets Vue and TypeScript infer the exact type of `item` in the parent when using `v-slot`.

### Usage in `App.vue`

The component is used three times with different data shapes. In each case, TypeScript knows the exact type of `item` inside the slot:

```vue
<!-- item is typed as MyNumberItem → item.stringValue is available -->
<ActivityList title="Some Number Items" :items="numberItems" v-slot="{ item }">
  {{ item.stringValue }}
</ActivityList>

<!-- item is typed as MixedItem → item.theValue is number | string -->
<ActivityList title="Some Mixed Items" :items="mixedItems" v-slot="{ item }">
  <code v-if="typeof item.theValue === 'number'">{{ item.theValue }}</code>
  <p v-else>{{ item.theValue }}</p>
</ActivityList>
```

## Tech stack

- **Vue 3.5** — Composition API, `<script setup>`
- **TypeScript 5.9**
- **Vite 7** — build tool and dev server
- **vue-tsc** — type checking in templates
- **ESLint + oxlint + oxfmt** — linting and formatting

## Scripts

```bash
npm install          # install dependencies
npm run dev          # start the development server
npm run build        # type-check + production build
npm run type-check   # type-check only
npm run lint         # lint (oxlint + eslint)
npm run format       # format source code
```
