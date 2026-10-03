---
description: >-
  This guide uses a movie selection application to demonstrate how core React
  components process layout modifications dynamically inside a user's browser.
icon: camera-movie
---

# Conceptual Reference: Interactive Movie Dashboard

````jsx
### 🧱 The Component Shell: `MovieCard.jsx`
Instead of duplicating code for every film, we build one reusable visual container card. It takes data parameters via **Props** and updates its memory layout using local **State**.

```jsx
// components/MovieCard.jsx
import { useState } from 'react';

/**
 * MovieCard displays an individual film entity.
 * Uses local state memory to track user watchlist selections.
 */
export default function MovieCard({ title, genre, year }) {
  // 1. Initialize local memory state (defaults to false / not added yet)
  const [isAdded, setIsAdded] = useState(false);

  return (
    <div className="movie-card border border-slate-200 p-5 bg-white rounded-xl shadow-sm max-w-xs mx-auto">
      <span className="text-xs font-bold text-emerald-600 bg-emerald-50 px-2 py-1 rounded">
        {genre}
      </span>
      <h3 className="text-lg font-bold text-slate-900 mt-2">{title}</h3>
      <p className="text-xs text-slate-400 mb-4">Released: {year}</p>

      {/* 2. State memory dynamically switches the visual styling of the action button */}
      <button
        onClick={() => setIsAdded(!isAdded)}
        className={`w-full p-2.5 rounded-lg text-sm font-semibold transition-colors ${
          isAdded 
            ? 'bg-slate-100 text-slate-700 hover:bg-slate-200' 
            : 'bg-slate-900 text-white hover:bg-slate-800'
        }`}
      >
        {isAdded ? "✓ Added to Watchlist" : "+ Add to Watchlist"}
      </button>
    </div>
  );
}
```

````
