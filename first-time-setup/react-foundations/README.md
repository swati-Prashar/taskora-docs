# React Foundations

## ⚛️ React Core Architecture Blueprint

This master engineering reference guide breaks down the core architecture of the React frontend framework ecosystem. It bridges conceptual client-side user interface (UI) rendering strategies with practical production-grade implementation rules.

***

### 🧱 Section 1: Components (The Modular Visual Bricks)

#### 💡 Core Concept

In a traditional web architecture, pages were built using massive, singular HTML files that mixed all structures together. React moves entirely away from this pattern by breaking the user interface down into isolated, self-contained, and reusable layout blocks called **Components**.

Think of a component like an independent Lego brick. The top navigation header is a component, a task item inside a dashboard grid is a component, and a status check icon is a component.

In pure technical terms, a React component is simply a standard JavaScript function that returns semantic visual layout markup using a special code extension language called **JSX (JavaScript XML)**.

#### 🛠️ Production Code Blueprint (MDX Multi-Framework Context)

Below is the structural anatomy of a modular Taskora component block. We use an MDX-style framework representation layer to declare our functional module:

```jsx
// components/TaskBadge.jsx
// An isolated UI component that paints a visual priority indicator on the screen

export default function TaskBadge() {
  return (
    <span className="badge-layout design-urgent animate-pulse">
      🚨 Action Required
    </span>
  );
}
```

#### 📋 Architectural Best Practices

* **The PascalCase Naming Standard:** React requires all custom component function names to begin strictly with a capital letter (e.g., `TaskBadge`, not `taskBadge`). This tells the layout compiler engine to parse it as a custom software component rather than a native browser HTML tag.
* **The Single Root Directive:** A component function must always return exactly **one root element**. If your UI needs to output two separate text nodes or divs side-by-side, they must be wrapped inside a single parent container tag or an empty React Fragment (`<> ... </>`).

***

### 🔌 Section 2: Props (Managing Dynamic Data Flow)

#### 💡 Core Concept

If every component brick were completely static, you would be forced to create hundreds of identical files just to show different words on a user screen. To solve this, components accept custom incoming parameters called **Props** (short for _properties_).

Think of Props like parameters passed down into a function, or the customized text engraved onto a blank label card. Props flow strictly in a **one-way downward direction**: from a parent container template down into individual child component blocks. A child block cannot naturally pass props back up to its parent.

#### 🛠️ Production Code Blueprint

Instead of hardcoding a text element, this component dynamically adapts its internal display parameters based on the custom values it receives from the database layer payload:

```jsx
// components/TaskHeader.jsx
// This component consumes a dynamic data prop parameter labeled "titleText"

export default function TaskHeader({ titleText }) {
  return (
    <div className="header-container-box">
      <h2 className="main-headline-text">{titleText}</h2>
    </div>
  );
}

// How an engineer calls this component inside a parent layout file:
// <TaskHeader titleText="Review Q4 Roadmap Specs" />
// <TaskHeader titleText="Verify Storyblok CDN Token" />
```

#### 📋 Architectural Best Practices

* **The Rule of Immutability:** Props are strictly **read-only**. A child component must never attempt to mutate, overwrite, or edit the properties data it receives from a parent layout stream. If the data needs to change dynamically, it belongs in **State**, not Props.
* **Destructuring Syntax:** We write incoming variables wrapped in object curly braces `{ titleText }` within our function parameters. This cleanly unpacks the parent property package instantly, keeping the layout files free of cluttered paths like `props.titleText`.

***

### 🔄 Section 3: State & Hook Lifecycles (Interactive Memory)

#### 💡 Core Concept

While Props are static inputs passed down from an outside system layer, **State** represents local memory. State is data that lives inside a component that can actively change over time based on real-time user behavior—such as typing words into a form, checking a task checkbox, or opening an input drawer.

To allow a standard functional component to remember information, we invoke a native React feature called a **Hook**, specifically the `useState` runtime tracker hook.

#### 🛠️ Production Code Blueprint

Below is a clean implementation model demonstrating how a local event cycle modifies state memory to re-render structural values on a monitor canvas:

```jsx
// components/TaskToggle.jsx
import { useState } from 'react';

export default function TaskToggle() {
  // 1. Declare state variable (isCompleted) and its explicit setter function (setIsCompleted)
  // The default value inside the hook parameter is explicitly set to false
  const [isCompleted, setIsCompleted] = useState(false);

  return (
    <div className="toggle-wrapper-box">
      <p>Current Execution State: <strong>{isCompleted ? "✅ Completed" : "⏳ Active Queue"}</strong></p>
      
      {/* 2. Execute state transformation loop on user interaction */}
      <button className="trigger-action-btn" onClick={() => setIsCompleted(!isCompleted)}>
        Toggle Complete State
      </button>
    </div>
  );
}
```

#### 📋 Architectural Best Practices

* **The Trigger for Re-Rendering:** Every single time a state variable is modified via its explicit setter function (e.g., `setIsCompleted`), React instantly triggers a **re-render lifecycle**. The framework recalculates the code output and seamlessly paints the new visual changes onto the screen without refreshing the browser page.
* **State Isolation Principle:** Local state memory is completely isolated. If you duplicate the `<TaskToggle />` component code block three times on a single web page layout, clicking the button on Card 1 will modify _only_ Card 1. Cards 2 and 3 remain completely unaffected, preserving independent component memory.
