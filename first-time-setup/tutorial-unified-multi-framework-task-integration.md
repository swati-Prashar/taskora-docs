---
icon: rocket
---

# Tutorial: Unified Multi-Framework Task Integration

```markdown
# 🚀 Tutorial: Unified Multi-Framework Task Integration

This tutorial guides you through building a dynamic task component across **Vue 3**, **React**, and **Next.js**. You will learn how to initialize local files, fetch live API text arrays, and understand how data crosses the client-server bridge.

---

## 🏛️ Phase 1: Understanding the Client-Server Bridge

Before writing code, look at how data travels over the internet from the Taskora cloud database down to your local app layout:


```

```markdown
[ Taskora Cloud Database ]
│
▼ (Ships raw JSON text array strings across the network)
[ Client-Server HTTP Line ]
│
▼ (Framework captures the payload data packet)
[ Your App UI Engine ] ➔ (Loops through the code and repaints the screen)
```

````markdown
Each framework intercepts this network data stream using a completely different architectural logic pattern:
1. **Vue 3** uses a client-side **Reactivity tracking hook (`ref`)** to continuously monitor and repaint data in the browser.
2. **React** uses a client-side **Immutability state function (`useState`)** to safely replace memory variables without breaking page layouts.
3. **Next.js** avoids browser loops completely, connecting to the data stream on the **Background Server** before the webpage ever leaves the cloud.

---

## 🛠️ Phase 2: Framework Implementation Tracks

Create a new file in your project components folder matching your chosen framework path below and mount the code block:

### Track A: Vue 3 (Composition API)
Vue coordinates browser actions using lifecycle listeners. We use `onMounted` to fetch data the exact millisecond the browser finish drawing the page shell.

```vue
<!-- components/TaskList.vue -->
<script setup>
import { ref, onMounted } from 'vue';

// 1. Set up live reactive memory switches inside the browser
const tasks = ref([]);
const isLoading = ref(true);

// 2. Intercept the server data packet automatically on page mount
onMounted(async () => {
  try {
    const res = await fetch('https://taskora.dev');
    const payload = await res.json();
    tasks.value = payload.data; // Mutate the memory block directly using .value
  } catch (err) {
    console.error('Vue integration failure:', err);
  } finally {
    isLoading.value = false; // Toggle the loading switch off
  }
});
</script>

<template>
  <div class="p-6 bg-slate-50 rounded-xl">
    <p v-if="isLoading" class="text-sm text-slate-500">Syncing with server nodes...</p>
    
    <!-- 3. Loop through memory records and append lists dynamically -->
    <ul v-else class="space-y-2">
      <li v-for="task in tasks" :key="task.id" class="p-3 bg-white border rounded shadow-sm">
        🟢 <strong>{{ task.title }}</strong> - status: {{ task.status }}
      </li>
    </ul>
  </div>
</template>
```

---

### Track B: Pure Client-Side React
React uses a strict data-flow architecture. To run network data downloads without re-triggering component rendering loops indefinitely, we isolate the fetch command using a `useEffect` hook.

```jsx
// components/ReactTaskList.jsx
'use client'; // Commands the builder to execute this file strictly in the user's browser

import { useState, useEffect } from 'react';

export default function ReactTaskList() {
  // 1. Enforce strict immutability boundaries using paired tracking hooks
  const [tasks, setTasks] = useState([]);
  const [isLoading, setIsLoading] = useState(true);

  // 2. Isolate network side-effects safely
  useEffect(() => {
    async function fetchDatabasePayload() {
      try {
        const res = await fetch('https://taskora.dev');
        const payload = await res.json();
        setTasks(payload.data); // Never mutate directly; pass a new array reference address
      } catch (err) {
        console.error('React runtime fetch failure:', err);
      } finally {
        setIsLoading(false);
      }
    }

    fetchDatabasePayload();
  }, []); // The empty dependency array [] forces this script to run exactly once

  if (isLoading) return <p className="text-sm text-slate-500">Loading React layout blocks...</p>;

  return (
    <ul className="p-6 bg-slate-50 rounded-xl space-y-2">
      {/* 3. Map memory arrays down to visual elements using standard JS iterators */}
      {tasks.map((task) => (
        <li key={task.id} className="p-3 bg-white border rounded shadow-sm">
          ⚛️ <strong>{{ task.title }}</strong> - status: {{ task.status }}
        </li>
      ))}
    </ul>
  );
}
```

---

### Track C: Next.js App Router (Server-Side Component)
Next.js completely flips the client-server bridge upside down. Instead of forcing the user's laptop to run network queries, the component executes directly on the server, rendering complete HTML text blocks before shipping code across the web.

```jsx
// app/tasks/page.jsx
// This file executes securely on the backend server with zero browser bundle weights

// 1. Secure server-side isolation data routine
async function queryCloudNodes() {
  const res = await fetch('https://taskora.dev');
  if (!res.ok) throw new Error('Network line failure communicating with database');
  return res.json();
}

export default async function NextTasksPage() {
  // 2. Direct server execution loop with zero risk of API token exposure
  const payload = await queryCloudNodes();

  return (
    <main className="p-8 bg-slate-50 min-h-screen max-w-md">
      <h1 className="text-xl font-bold text-slate-900 mb-4">Next.js Server Context</h1>
      
      {/* 3. Render raw compiled server strings down onto the canvas elements instantly */}
      <ul className="space-y-2">
        {payload.data.map((task) => (
          <li key={task.id} className="p-3 bg-white border rounded shadow-sm">
            🚀 <strong>{task.title}</strong> - status: {task.status}
          </li>
        ))}
      </ul>
    </main>
  );
}
```
````
