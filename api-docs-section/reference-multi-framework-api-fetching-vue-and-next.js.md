---
icon: plug
---

# Reference: Multi-Framework API Fetching (Vue & Next.js)

````markdown
# 🔌 Reference: Multi-Framework API Fetching (Vue & Next.js)

This technical reference details how to fetch structural data arrays from the Taskora REST API gateway and safely consume payloads using both **Vue 3 (Composition API)** and **Next.js App Router** logic models.

---

## 🌐 The Data Endpoint
👉 **`GET https://taskora.dev`**

### Expected JSON Response Payload Shape
```json
{
  "status": "success",
  "data": [
    {
      "id": "task_101",
      "title": "Document Framework Routing Boundaries",
      "status": "active"
    }
  ]
}
```

---

## 💚 Implementation 1: Vue 3 (Composition API)
In the Vue architecture model, we handle asynchronous data fetching inside browser memory using lifecycle events (`onMounted`) and track visual reactive state switches using the `ref()` tracker wrapper.

```vue
<!-- components/TaskList.vue -->
<script setup>
import { ref, onMounted } from 'vue';

// 1. Initialize dynamic reactive state memory arrays
const tasks = ref([]);
const isLoading = ref(true);

// 2. Asynchronous execution loop to query the background REST endpoint
onMounted(async () => {
  try {
    const res = await fetch('https://taskora.dev');
    const payload = await res.json();
    tasks.value = payload.data; // Assign text arrays to Vue reactive nodes
  } catch (err) {
    console.error('API integration channel failure:', err);
  } finally {
    isLoading.value = false;
  }
});
</script>

<template>
  <div class="task-grid p-6 bg-slate-50 rounded-xl">
    <p v-if="isLoading" class="text-sm text-slate-500">Retrieving system nodes...</p>
    
    <!-- 3. Dynamic layout loop parsing template blocks cleanly -->
    <ul v-else class="space-y-3">
      <li v-for="task in tasks" :key="task.id" class="p-3 bg-white shadow-xs rounded border border-slate-200">
        💼 <span class="font-bold text-slate-900">{{ task.title }}</span> - {{ task.status }}
      </li>
    </ul>
  </div>
</template>
```

---

## 🚀 Implementation 2: Next.js App Router (Server-Side)
Unlike Vue's client-side browser lifecycle execution loops, Next.js executes asynchronous network requests directly on the **secure background server** by default. This wipes out heavy client-side bundle download weights.

```jsx
// app/tasks/page.jsx

// 1. Secure server-side isolation data fetching routine
async function getWorkspaceTasks() {
  const res = await fetch('https://taskora.dev', {
    next: { revalidate: 60 } // Securely cache network data text tokens for 60 seconds
  });
  
  if (!res.ok) throw new Error('Failed to resolve database transport lines');
  return res.json();
}

export default async function TasksPage() {
  // 2. Direct server execution loop with zero risk of public API token exposure
  const payload = await getWorkspaceTasks();

  return (
    <main className="p-8 bg-slate-50 min-h-screen">
      <h1 className="text-2xl font-bold text-slate-900 mb-4">Taskora Cloud Nodes</h1>
      
      {/* 3. Render arrays down into semantic layout maps instantly */}
      <ul className="space-y-3 max-w-lg">
        {payload.data.map((task) => (
          <li key={task.id} className="p-3 bg-white shadow-xs rounded border border-slate-200">
            💼 <span class="font-bold text-slate-900">{task.title}</span> - {task.status}
          </li>
        ))}
      </ul>
    </main>
  );
}
```

````
