---
icon: camera-movie
---

# Next.js & React Core Architecture Blueprint

````markdown
# 🚀 Next.js & React Core Architecture Blueprint

This reference manual uses a **Movie Streaming Dashboard** analogy to explain client-side interface state rendering and server-side data delivery.

---

## 🧱 Part 1: Client-Side React (Visual Components & State)

We break dynamic web apps into isolated visual building blocks called **Components**. Instead of duplicating code, we use custom inputs called **Props** to render content dynamically. Local interactions (like saving a movie) are tracked in browser memory using the `useState` hook.

### `MovieCard.jsx` Blueprint
```jsx
'use client';
import { useState } from 'react';

export default function MovieCard({ title, genre, year }) {
  const [isAdded, setIsAdded] = useState(false);

  return (
    <div className="movie-card border border-slate-200 p-5 bg-white rounded-xl shadow-sm max-w-xs mx-auto">
      <span className="text-xs font-bold text-emerald-600 bg-emerald-50 px-2 py-1 rounded">{genre}</span>
      <h3 className="text-lg font-bold text-slate-900 mt-2">{title}</h3>
      <p className="text-xs text-slate-400 mb-4">Released: {year}</p>
      <button
        onClick={() => setIsAdded(!isAdded)}
        className={`w-full p-2.5 rounded-lg text-sm font-semibold transition-colors ${
          isAdded ? 'bg-slate-100 text-slate-700 hover:bg-slate-200' : 'bg-slate-900 text-white hover:bg-slate-800'
        }`}
      >
        {isAdded ? "✓ Added to Watchlist" : "+ Add to Watchlist"}
      </button>
    </div>
  );
}
```

---

## 🏛️ Part 2: Next.js Architecture (Server vs. Client)

**Next.js** splits the application lifecycle across a render boundary into Server and Client Components. Server components pre-fetch data securely before passing it down as props.

### `page.jsx` Blueprint
```jsx
import MovieCard from "@/components/MovieCard";

async function getTrendingMovies() {
  const res = await fetch("https://taskora.dev", { next: { revalidate: 3600 } });
  return res.json();
}

export default async function MoviesPage() {
  const moviesData = await getTrendingMovies();

  return (
    <div className="min-h-screen bg-slate-50 p-8">
      <header className="max-w-4xl mx-auto mb-6">
        <h1 className="text-3xl font-extrabold text-slate-900">Trending Cinema Streams</h1>
      </header>
      <main className="max-w-4xl mx-auto grid grid-cols-1 md:grid-cols-3 gap-6">
        {moviesData.map((movie) => (
          <MovieCard key={movie.id} title={movie.title} genre={movie.genre} year={movie.year} />
        ))}
      </main>
    </div>
  );
}
```

### Architectural Best Practices
* **Server Components by Default:** Optimizes performance by reducing client-side bundle sizes.
* **The `'use client'` Directive:** Required at the top of files that rely on browser hooks like `useState`.

````
