# Templating

## Text Interpolation

```vue
<template>
  <div>
    <p>{{ message }}</p>
    <p>{{ num + 13 }}</p>
    <p>{{ ok ? "YES" : "NO" }}</p>
    <p>{{ message.split("").reverse().join("") }}</p>
  </div>
</template>
```

## Directives

```vue
<template>
  <div>
    <!-- Dynamic Attribute Binding -->
    <div v-bind:id="dynamicId"></div>
    <div :id="dynamicId"><!-- shorthand --></div>
    <div :id><!-- :id="id" --></div>

    <!-- Event Handling -->
    <button v-on:click="handleClick()">Click Me</button>
    <button @click="handleClick()">Click Me</button>

    <!-- Dynamic Arguments -->
    <div v-bind:[dynamicAttr]="value"></div>

    <!-- Conditional Rendering -->
    <div v-if="isVisible">This is visible</div>
    <div v-else-if="another">Else-if chain</div>
    <div v-else>This is hidden</div>

    <!-- Conditional Visibility -->
    <div v-show="isVisible">This is visible</div>

    <!-- List Rendering -->
    <ul>
      <li v-for="item in items" :key="item.id">{{ item.name }}</li>
      <li v-for="(item, index) in items" :key="index">{{ item.name }}</li>
    </ul>
  </div>
</template>
```

## Raw HTML

```admonish warning
Dynamically rendering arbitrary HTML on your website can be very dangerous because
it can easily lead to **XSS vulnerabilities**. Only use v-html on trusted content
and never on user-provided content.
```

```vue
<template>
  <div>
    <p>Using text interpolation: {{ rawHtml }}</p>
    <p>Using v-html directive: <span v-html="rawHtml"></span></p>
  </div>
</template>

<script setup lang="ts">
const rawHtml = "<strong>This is raw HTML</strong>";
</script>
```
