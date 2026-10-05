---
icon: plug
---

# Reference: Multi-Framework API Fetching (Vue, React, Next.js)

````markdown
# 🔌 Reference: Multi-Framework API Fetching (Vue, React, Next.js)

This technical reference details how to fetch structural data arrays from the Taskora REST API gateway and safely consume payloads across **Vue 3**, **React**, and **Next.js App Router** logic models.

---

## 🌐 The Shared Data Endpoint

All frontend framework integration streams query the exact same global database node:
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

## 💚 1. Vue 3 (Composition API)

In the Vue architecture model, we handle asynchronous data fetching inside browser memory using lifecycle events (`onMounted`) and track visual reactive state switches using the `ref()` tracker wrapper.

```vue
<!-- components/TaskList.vue -->
<script setup>
import { ref, onMounted } from 'vue';

// Initialize dynamic reactive state memory arrays
const tasks = ref([]);
const isLoading = ref(true);

// Asynchronous execution loop to query the background REST endpoint
onMounted(async () => {
  try {
    const res = await fetch('https://taskora.dev');
    const payload = await res.json();
    tasks.value = payload.data; 
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
    
    <ul v-else class="space-y-3">
      <li v-for="task in tasks" :key="task.id" class="p-3 bg-white shadow-sm rounded border border-slate-200">
        💼 <span class="font-bold text-slate-900">{{ task.title }}</span> - {{ task.status }}
      </li>
    </ul>
  </div>
</template>
```

---

## ⚛️ 2. Pure Client-Side React

In a standard client-side React component, we isolate network side-effects using the `useEffect` hook and enforce immutability boundaries using local memory tracking states (`useState`).

```jsx
// components/ReactTaskList.jsx
'use client';

import { useState, useEffect } from 'react';

export default function ReactTaskList() {
  // Initialize local component memory states
  const [tasks, setTasks] = useState([]);
  const [isLoading, setIsLoading] = useState(true);

  useEffect(() => {
    async function downloadTasks() {
      try {
        const res = await fetch('https://taskora.dev');
        const payload = await res.json();
        setTasks(payload.data); // Safely pass a new data object reference to state
      } catch (err) {
        console.error('React data fetch crash:', err);
      } finally {
        setIsLoading(false);
      }
    }
    
    downloadTasks();
  }, []); // Empty dependency array ensures this fires exactly once on mount

  if (isLoading) return <p className="text-sm text-slate-500">Loading React nodes...</p>;

  return (
    <ul className="space-y-3 p-6 bg-slate-50 rounded-xl">
      {tasks.map((task) => (
        <li key={task.id} className="p-3 bg-white shadow-sm rounded border border-slate-200">
          💼 <span className="font-bold text-slate-900">{task.title}</span> - {task.status}
        </li>
      ))}
    </ul>
  );
}
```

---

## 🚀 3. Next.js App Router (Server-Side)

Unlike Vue or basic React client components, Next.js executes asynchronous network requests directly on the **secure background server** by default. This wipes out heavy client-side bundle download weights.

```jsx
// app/tasks/page.jsx

// Secure server-side isolation data fetching routine
async function getWorkspaceTasks() {
  const res = await fetch('https://taskora.dev', {
    next: { revalidate: 60 } // Securely cache network data text tokens for 60 seconds
  });
  
  if (!res.ok) throw new Error('Failed to resolve database transport lines');
  return res.json();
}

export default async function TasksPage() {
  // Direct server execution loop with zero risk of public API token exposure
  const payload = await getWorkspaceTasks();

  return (
    <main className="p-8 bg-slate-50 min-h-screen">
      <h1 className="text-2xl font-bold text-slate-900 mb-4">Taskora Cloud Nodes</h1>
      
      <ul className="space-y-3 max-w-lg">
        {payload.data.map((task) => (
          <li key={task.id} className="p-3 bg-white shadow-sm rounded border border-slate-200">
            💼 <span class="font-bold text-slate-900">{task.title}</span> - {task.status}
          </li>
        ))}
      </ul>
    </main>
  );
}
```

````
