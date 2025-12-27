# Vue.js Style Guide Summary

This document summarizes key rules and best practices from the official Vue.js Style Guide.

## 1. Component Structure (Vue 3 Composition API)
- **File Extension:** `.vue` for Single File Components (SFC)
- **Naming:** `PascalCase` for component names in both file names and registration
- **Single File Components:** Use SFC format with `<template>`, `<script>`, and `<style>` sections

```vue
<!-- Good: UserProfile.vue -->
<script setup>
import { ref, computed } from 'vue';

const props = defineProps({
  userId: {
    type: String,
    required: true
  }
});

const user = ref(null);
const fullName = computed(() => `${user.value?.firstName} ${user.value?.lastName}`);
</script>

<template>
  <div class="user-profile">
    <h1>{{ fullName }}</h1>
  </div>
</template>

<style scoped>
.user-profile {
  padding: 20px;
}
</style>
```

## 2. Component Naming Conventions (Priority A: Essential)
- **Multi-word Component Names:** Always use multi-word names (except root `App`)
- **Base Components:** Prefix with `Base`, `App`, or `V` (e.g., `BaseButton`, `AppButton`)
- **Single-Instance Components:** Prefix with `The` (e.g., `TheHeader`, `TheSidebar`)
- **Tightly Coupled Components:** Child components should include parent name (e.g., `TodoList`, `TodoListItem`)

```
components/
  BaseButton.vue
  BaseIcon.vue
  TheHeader.vue
  TheSidebar.vue
  TodoList.vue
  TodoListItem.vue
  TodoListItemButton.vue
```

## 3. Props (Priority A: Essential)
- **Detailed Definitions:** Always define props with types, required status, validators
- **Naming:** Use camelCase in JavaScript, kebab-case in templates
- **Types:** Specify prop types (String, Number, Boolean, Array, Object, Function)

```vue
<script setup>
defineProps({
  status: {
    type: String,
    required: true,
    validator: (value) => ['draft', 'published', 'archived'].includes(value)
  },
  title: {
    type: String,
    default: 'Untitled'
  },
  likes: {
    type: Number,
    default: 0
  }
});
</script>

<template>
  <!-- Use kebab-case in templates -->
  <BlogPost
    :post-title="title"
    :like-count="likes"
    status="published"
  />
</template>
```

## 4. Composition API (Vue 3)
- **`<script setup>`:** Prefer `<script setup>` for cleaner syntax
- **Reactive State:** Use `ref()` for primitives, `reactive()` for objects
- **Computed Properties:** Use `computed()` for derived state
- **Lifecycle Hooks:** Use `onMounted()`, `onUnmounted()`, etc.

```vue
<script setup>
import { ref, computed, onMounted } from 'vue';

const count = ref(0);
const doubled = computed(() => count.value * 2);

onMounted(() => {
  console.log('Component mounted');
});

function increment() {
  count.value++;
}
</script>
```

## 5. Template Directives
- **v-for with key:** Always use `:key` with `v-for`
- **v-if with v-for:** Never use together on same element (Priority A)
- **Directive Shorthand:** Use `:` for `v-bind`, `@` for `v-on`
- **Self-Closing Components:** Always self-close components with no content

```vue
<template>
  <!-- Good: key with v-for -->
  <li v-for="user in users" :key="user.id">
    {{ user.name }}
  </li>

  <!-- Good: v-if on wrapper -->
  <ul v-if="shouldShowUsers">
    <li v-for="user in users" :key="user.id">
      {{ user.name }}
    </li>
  </ul>

  <!-- Good: shorthand -->
  <button @click="handleClick" :disabled="isLoading">
    Click me
  </button>

  <!-- Good: self-closing -->
  <BaseIcon />
</template>
```

## 6. Component Communication
- **Props Down:** Pass data down via props
- **Events Up:** Emit events to parent with `defineEmits`
- **Provide/Inject:** For deeply nested components
- **Pinia/Vuex:** For global state management

```vue
<script setup>
const emit = defineEmits(['update', 'delete']);

function handleUpdate(data) {
  emit('update', data);
}

function handleDelete(id) {
  emit('delete', id);
}
</script>

<template>
  <button @click="handleUpdate(formData)">Update</button>
  <button @click="handleDelete(itemId)">Delete</button>
</template>
```

## 7. Scoped Styles (Priority B: Strongly Recommended)
- **Scoped Styles:** Always use `scoped` attribute for component styles
- **BEM or Other Convention:** Use consistent naming conventions
- **Deep Selectors:** Use `:deep()` for styling child components

```vue
<style scoped>
.user-profile {
  padding: 20px;
}

.user-profile__header {
  font-size: 24px;
}

/* Style child component */
:deep(.child-component) {
  color: blue;
}
</style>
```

## 8. Component Options Order (Priority C: Recommended)
Order for Options API (if used):
1. Global Awareness: `name`, `components`
2. Template Dependencies: `directives`, `components`
3. Composition: `extends`, `mixins`, `provide/inject`
4. Interface: `props`, `emits`
5. Local State: `data`, `computed`
6. Events: `watch`, lifecycle hooks
7. Methods: `methods`

## 9. Composables (Custom Composition Functions)
- **Naming:** Prefix with `use` (e.g., `useUser`, `useFetch`)
- **File Location:** Store in `composables/` directory
- **Return Values:** Return reactive state and methods

```javascript
// composables/useUser.js
import { ref, computed } from 'vue';

export function useUser() {
  const user = ref(null);
  const isLoggedIn = computed(() => user.value !== null);

  async function login(credentials) {
    user.value = await api.login(credentials);
  }

  function logout() {
    user.value = null;
  }

  return {
    user,
    isLoggedIn,
    login,
    logout
  };
}
```

## 10. File Structure
```
src/
  components/
    base/          # Base components (BaseButton, BaseInput)
    common/        # Shared components
    features/      # Feature-specific components
  composables/     # Composition functions
  stores/          # Pinia stores
  views/           # Page components (routing)
  router/          # Vue Router configuration
  assets/          # Static assets
```

## 11. Best Practices
- **Template Expressions:** Keep simple, extract complex logic to computed
- **Component Data:** `data` must be a function (Options API)
- **Avoid `this` in Composition API:** Use refs and reactive
- **TypeScript:** Use TypeScript for type safety
- **Testing:** Use Vue Test Utils and Vitest

```vue
<script setup>
// Good: Simple template expressions
const formattedDate = computed(() => formatDate(createdAt.value));
</script>

<template>
  <!-- Good: Use computed -->
  <span>{{ formattedDate }}</span>

  <!-- Bad: Complex logic in template -->
  <span>{{ new Date(createdAt).toLocaleDateString('en-US') }}</span>
</template>
```

**BE CONSISTENT.** Follow the official Vue Style Guide and use ESLint with vue-eslint plugin.

*Source: [Official Vue.js Style Guide](https://vuejs.org/style-guide/)*
