---
icon: react
---

# React Core Architecture Blueprint

## React Component Data Architecture: The Appointment System

This developer reference guide details how to construct a component-driven **Appointment Booking Interface** by connecting decoupled headless CMS properties cleanly into modular React layout nodes.

***

### 🧱 Section 1: Components (The Content Layout Blocks)

#### 💡 The Conceptual Analogy

Inside a modern headless content management architecture, digital teams compose application screens by stacking structured, modular data blocks. In the frontend application layer, an engineer maps each data schema directly to an isolated **React Component**.

Think of a component like a self-contained structural shell template. Below, our `BookingCard` component acts as the parent blueprint container. It maps the CMS text parameters directly into a clean, reusable visual user interface frame.

#### 🛠️ Production Code Blueprint: `BookingCard.jsx`

```jsx
// components/BookingCard.jsx

/**
 * BookingCard serves as the parent container layout block.
 * Binds the incoming content properties to a clean visual UI panel structure.
 */
export default function BookingCard({ contentPayload, children }) {
  return (
    <div className="appointment-container border border-slate-200 rounded-xl p-6 bg-white max-w-sm mx-auto shadow-sm">
      {/* Renders the dynamic headline and descriptive copy managed inside the CMS fields */}
      <h3 className="text-lg font-bold text-slate-900 mb-2">{contentPayload.Headline}</h3>
      <p className="text-sm text-slate-500 mb-4">{contentPayload.Description_Text}</p>
      
      {/* Yields layout space dynamically to child time-slot components */}
      <div className="time-slots-wrapper space-y-3">
        {children}
      </div>
    </div>
  );
}
```

***

### 🔌 Section 2: Props (Passing Immutable Content Data)

#### 💡 The Conceptual Analogy

If you had to build a brand-new component file from scratch every time your team wanted to offer a new appointment hour, your frontend application codebase would quickly become unmanageable. Instead, we use **Props** to pass dynamic, changing field strings downstream.

Think of Props like individual content inputs typed out inside an editorial fields dashboard. When a manager inputs `"09:00 AM"` or `"02:30 PM"`, the API handles that string as an immutable data packet. The single, reusable frontend button template consumes those properties and paints them dynamically on the screen.

#### 🛠️ Production Code Blueprint: `TimeSlot.jsx`

```jsx
// components/TimeSlot.jsx

/**
 * TimeSlot accepts immutable property metrics passed from a parent mapping loop.
 */
export default function TimeSlot({ timeLabel, isAvailable }) {
  return (
    <button 
      disabled={!isAvailable}
      className={`w-full p-3 rounded-lg text-sm font-semibold border text-center transition-all
        ${isAvailable 
          ? 'bg-slate-50 border-slate-200 text-slate-800 hover:bg-slate-900 hover:text-white' 
          : 'bg-slate-100 border-slate-100 text-slate-400 cursor-not-allowed'
        }`}
    >
      {timeLabel} {!isAvailable && '(Reserved)'}
    </button>
  );
}

// How properties map data downstream linearly:
// <TimeSlot timeLabel="10:00 AM" isAvailable={true} />
// <TimeSlot timeLabel="01:00 PM" isAvailable={false} />
```

***

### 🔄 Section 3: State & Hooks (Local Application Memory)

#### 💡 The Conceptual Analogy

A headless CMS is fantastic for storing content fields that rarely change throughout the day (like titles or general time labels). However, when a user clicks a button to instantly confirm an appointment reservation, that rapid click interaction doesn't belong inside a cloud content database—it belongs in the user browser’s local application memory loop. We call this **State**.

We use the React hook `useState` to initialize local memory. The moment the user triggers the click button event, the component instantly updates its internal memory flag and re-renders the visual markup dynamically without forcing the browser to reload the page.

#### 🛠️ Production Code Blueprint: `BookingConfirmation.jsx`

```jsx
// components/BookingConfirmation.jsx
import { useState } from 'react';

/**
 * BookingConfirmation manages local transactional states independently from backend database layers.
 */
export default function BookingConfirmation() {
  // 1. Initialize local state variable. The default state is false.
  const [isReserved, setIsReserved] = useState(false);

  return (
    <div className="status-tracker-box p-4 rounded-lg bg-slate-50 text-center border border-dashed border-slate-200">
      {/* 2. Conditionally reads current state memory to swap out visual UI rendering trees */}
      {isReserved ? (
        <div className="text-emerald-600 font-medium animate-fade-in">
          🎉 Session Secured Successfully! Check your email inbox for tracking confirmations.
        </div>
      ) : (
        <div>
          <p className="text-sm text-slate-600 mb-3">Your selected time slot is held securely for 5 minutes.</p>
          
          {/* 3. Event handler catches interaction loop, triggering the setter to flip state to true */}
          <button 
            onClick={() => setIsReserved(true)}
            className="w-full bg-emerald-600 hover:bg-emerald-700 text-white font-semibold text-sm p-2.5 rounded-lg transition-colors shadow-sm"
          >
            Confirm Reservation
          </button>
        </div>
      )}
    </div>
  );
}
```
