# 🗳️ NcCombobox for Nextcloud

[![License: MIT or Artistic-2.0](https://img.shields.io)](LICENSE)
[![Vue 3](https://img.shields.io)](https://vuejs.org)
[![Nextcloud Vue 9](https://img.shields.io)](https://github.com)

A high-performance, Nextcloud-native **Combobox** component. While standard select components often struggle with free-text input, `NcCombobox` treats the user's typing as the primary source of truth, offering suggestions without restricting the input.

---

## 🔍 The Problem it Solves

Standard components like `NcSelectTags` are great for selecting existing entities but often fail in "search-as-you-type" or "free-form" scenarios:
1. **Selection Bias:** They often clear the input if the user blurs the field without selecting an existing option.
2. **Delayed Sync:** `v-model` updates often only trigger on final selection, not on every keystroke.
3. **"Alien" Look:** Custom implementations often miss the specific Nextcloud focus rings, shadows, and z-index behaviors.

**NcCombobox** fixes this by wrapping `@nextcloud/vue/NcSelect` with a specialized configuration for real-time applications.

---

## ✨ Features

- **⚡ Real-time Emission:** Updates `v-model` on every single keystroke (`@search-change`).
- **📝 Persistent Free-text:** Uses `taggable` and `push-tags` logic to ensure typed text stays active, even if it's not in the `options` list.
- **🎨 Native Nextcloud UI:** Inherits all CSS variables and styles from the official `@nextcloud/vue` library.
- **♿ Accessible:** Full keyboard navigation support (Up/Down/Enter/Esc) inherited from the core.
- **🏗️ Developer Friendly:** Built for **Vue 3** and **Vite** environments.

---

## 🚀 Installation & Setup

Since this is a reference implementation, you can either install it as a dependency (if published) or drop the SFC into your project.

### 1. Requirements
- `@nextcloud/vue >= 9.0.0`
- `vue >= 3.0.0`

### 2. Usage example

```vue
<script setup>
import { ref } from 'vue'
import NcCombobox from './components/NcCombobox.vue'

const searchQuery = ref('')
const myOptions = ['Nextcloud Hub', 'Talk', 'Groupware', 'Files']

const handleUpdate = (val) => {
    console.log('Current Input:', val)
}
</script>

<template>
    <NcCombobox
        v-model="searchQuery"
        :options="myOptions"
        label="Search system"
        placeholder="Type to search or create..."
        @update:modelValue="handleUpdate" />
</template>
```

