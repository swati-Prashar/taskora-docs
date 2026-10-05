---
icon: file
---

# Real-Time Previews (Visual Editor Bridge)

````markdown
🔄 Real-Time Previews (Visual Editor Bridge)

This guide outlines how the frontend application communicates directly with the Headless CMS Visual Editor using a secure preview iframe bridge and real-time event listeners.

### 💡 The Conceptual Analogy
Think of the Visual Editor as a parent window holding your website inside a hidden touchscreen frame (an HTML `<iframe>`). When a content creator types a new headline inside the sidebar dashboard, the CMS doesn't save to the main database yet. Instead, it transmits a live message across the iframe boundary. 

A special React hook in your frontend application listens for this event, intercepts the updated text packet, and instantly repaints the screen in milliseconds so the author sees changes live before hitting publish.

### 🛠️ Production Code Blueprint: The Live Preview Bridge
```jsx
// hooks/useLivePreview.js
'use client';

import { useState, useEffect } from 'react';
import { registerStoryblokBridge, useStoryblokState } from '@storyblok/react';

/**
 * Initializes real-time editing event listeners inside the user browser.
 * Intercepts iframe messages from the CMS dashboard to trigger immediate UI re-renders.
 */
export default function useLivePreview(initialStoryData) {
  // 1. Initialize local state with initial server-fetched content data payload
  const [story, setStory] = useState(initialStoryData);

  useEffect(() => {
    // 2. Safely attach event listeners to intercept incoming postMessage strings from the CMS iframe
    registerStoryblokBridge(
      story.id,
      (newStory) => setStory(newStory), // 3. Callback updates state memory instantly on text change events
      {
        customParent: "https://storyblok.com",
        preventClick: true // Prevents standard link clicks from breaking the preview panel navigation
      }
    );
  }, [story.id]);

  return story;
}
```

````
