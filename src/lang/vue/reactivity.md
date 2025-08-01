# Reactivity

## `ref()`

`ref()` wraps a value in a reactive object, allowing Vue to track changes to that
value and update the DOM accordingly. This is the recommended way to create
reactive primitive values. `shallowRef()` is the shallow version of `ref()`, which
opt-out of deep reactivity for the value it holds.

```vue
<template>
  <div>
    <p>Count is: {{ count }}</p>
    <button @click="count++">Increment</button>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";

const count = ref(0);

console.log(count); // { value: 0 }
console.log(count.value); // 0

count.value++;
console.log(count.value); // 1
</script>
```

## `reactive()`

`reactive()` creates a reactive object from a plain JavaScript object. It
makes an object itself reactive. `shallowReactive()` is the shallow version of
`reactive()`, which opt-out of deep reactivity for the properties it holds.

```vue
<template>
  <div>
    <button @click="state.count++">{{ state.count }}</button>
  </div>
</template>

<script setup lang="ts">
import { reactive } from "vue";

const state = reactive({ count: 0 });
</script>
```

It is important to note that the returned value from `reactive()` is a
[Proxy](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy)
of the original object, which is not equal to the original object. Only the proxy
is reactive - mutating the original object will not trigger updates.

## `computed()`

```vue
<script setup>
import { reactive, computed } from "vue";

const author = reactive({
  name: "John Doe",
  books: [
    "Vue 2 - Advanced Guide",
    "Vue 3 - Basic Guide",
    "Vue 4 - The Mystery",
  ],
});

// a computed ref
const publishedBooksMessage = computed(() => {
  return author.books.length > 0 ? "Yes" : "No";
});
</script>
```

## `watch()`

```vue
<script setup>
import { ref, watch } from "vue";

const question = ref("");
const answer = ref("Questions usually contain a question mark. ;-)");
const loading = ref(false);

// watch works directly on a ref
watch(question, async (newQuestion, oldQuestion) => {
  if (newQuestion.includes("?")) {
    loading.value = true;
    answer.value = "Thinking...";
    try {
      const res = await fetch("https://yesno.wtf/api");
      answer.value = (await res.json()).answer;
    } catch (error) {
      answer.value = "Error! Could not reach the API. " + error;
    } finally {
      loading.value = false;
    }
  }
});
</script>

<template>
  <p>
    Ask a yes/no question:
    <input v-model="question" :disabled="loading" />
  </p>
  <p>{{ answer }}</p>
</template>
```
