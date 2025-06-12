# Vue

- [Official Website](https://vuejs.org/)

```vue
<!-- Single File Component -->
<template>
  <button @click="count++">Count is: {{ count }}</button>
</template>

<script setup lang="ts">
// Composition API
import { ref } from "vue";
const count = ref(0);
</script>

<style scoped>
button {
  font-weight: bold;
}
</style>
```

## Single File Components (SFC)

A Vue component is typically defined in a single file with the `.vue` extension,
which contains three sections:

- `<template>`: HTML-like syntax for the component's structure.
- `<script>`: JavaScript (or TypeScript) code for the component's logic.
- `<style>`: CSS styles for the component, which can be scoped to avoid conflicts.
