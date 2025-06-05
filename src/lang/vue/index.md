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
