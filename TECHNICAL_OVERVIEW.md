# Technical Implementation Overview

## 📘 Project Scope
The **Task Management Dashboard** is a high-performance, client-side application designed for efficiency and simplicity. This document outlines the technical architecture, data flow, and component structure of the system.

## 🏗️ System Architecture
The application follows a **Monolithic Client-Side Architecture** where the structure (HTML), style (CSS), and logic (JS) are tightly integrated but clearly separated by concern.

```mermaid
graph TD
    subgraph Client_Layer [Client / Browser]
        User[User Interaction]
        DOM[DOM / UI Layer]
    end

    subgraph Logic_Layer [Application Logic]
        Script[Script.js]
        Val[Input Validation]
        CRUD[CRUD Operations]
    end

    subgraph Data_Layer [Data & Assets]
        Mem[In-Memory Storage]
        Assets[Image Assets]
    end

    User -->|Clicks / Types| DOM
    DOM -->|Trigger Events| Script
    Script -->|Validates| Val
    Script -->|Modifies| CRUD
    CRUD -->|Updates| Mem
    CRUD -->|Renders| DOM
    DOM -.->|Loads| Assets
```

---

## ⚡ Task Lifecycle Flow
The following flowchart illustrates the logical execution path when a user adds and manages a new task.

```mermaid
flowchart LR
    Start([User Input]) --> Check{Is Empty?}
    
    Check -- Yes --> Alert[No Action / Alert]
    Check -- No --> Create[Create &lt;div&gt; Element]
    
    Create --> SetClass[Set Class: 'task-item']
    SetClass --> InjectHTML[Inject Inner HTML]
    InjectHTML --> BindEvents[Bind Edit/Delete Events]
    
    BindEvents --> Append[Append to #taskList]
    Append --> UpdateDOM[Update Visualization]
    UpdateDOM --> End([Task Visible])
```

---

## 📂 Component Breakdown
A detailed breakdown of the file structure and the responsibility of each key component.

| Component | Type | Responsibility | Key Functions / Selectors |
| :--- | :--- | :--- | :--- |
| **Task.html** | `Structure` | Defines the DOM topology, including the dashboard grid, charts container, and task list area. | `#taskInput`, `#taskList`, `.dashboard` |
| **Script.js** | `Controller` | Acts as the central event controller. Handles user input, manipulates the DOM, and manages ephemeral state. | `addTask()`, `editTask()`, `deleteTask()` |
| **Style Blocks** | `Presentation` | Manages visual hierarchy, responsive layouts, and state indicators (e.g., overdue colors). | `.task-card`, `.sidebar`, `.header` |
| **Images/** | `Assets` | Contains static optimized assets for UI icons and graph visualizations. | `editing.png`, `bin.png`, `Graph 1.png` |

## 📊 Data & State Management
Currently, the application operates with **Ephemeral State Persistence**.
*   **Storage scope:** Session-based (Refresh clears data)
*   **Data Structure:** DOM-bound (State is reflected directly in the HTML Tree)
*   **Complexity:** O(1) for Add, O(1) for Delete (Direct Reference)
