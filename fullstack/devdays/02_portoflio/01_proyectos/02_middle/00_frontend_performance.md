
![[Pasted image 20250624160501.png]]


1. Selective Rendering  
🔦Purpose: Display only visible elements to improve rendering performance.  
🔎Strategy:  
✔️Above the fold: Elements that are visible before scrolling should be rendered immediately.  
✔️Below the fold: Elements outside the initial viewport can be deferred or loaded later.  
  
2. Code Splitting  
🔦Purpose: Divide a large application bundle into smaller, manageable parts for faster loading.  
🔎Details:  
✔️Specifically targets large JS bundles, like app.js (5 MB here).  
📢Findings:  
✔️home.js: 1.5 MB  
✔️products.js: 3 MB  
✔️about.js: 0.5 MB  
✔️Benefit: Improves initial load time and overall performance by loading only what’s necessary.  
  
3. Compression  
🔦Purpose: Reduce file sizes before transmission over the network.  
🔎Method: Use compression algorithms (like gzip, Brotli) on files.  
✔️Example: This could reduce payload size (e.g., large JS, CSS, images).  
  
4. Dynamic Imports  
🔦Purpose: Load code modules on demand rather than at initial load.  
🔎How:  
✔️Use JavaScript import() to load modules dynamically.  
📢Conditions: based on user actions or other triggers.  
🎤Example: When the user accesses a part of the website, only then load the associated code.  
  
5. Pre-fetching  
🔦Purpose: Fetch resources preemptively before they are needed.  
🔎Method:  
✔️Use <link rel="preload">, <link rel="prefetch">, or similar techniques.  
✔️Aims to fetch resources like pages or assets in the background.  
🎤Example: Pre-fetching the next page or component before the user navigates there.  
  
6. Loading Sequence  
🔦Purpose: Prioritize the critical resources that need to be loaded first for better user experience.  
🔎Sequence:  
✔️HTML (structure of the page)  
✔️CSS (styling)  
✔️JS (interactivity)  
🎤Note: Loading these resources in the right order ensures that the page renders progressively and quickly.  
  
7. Priority-Based Loading  
🔦Purpose: Load important resources first, deferring less critical ones.  
🔎Implementation:  
✔️Critical resources (like the main HTML, essential CSS, and important JS) are prioritized.  
✔️Non-essential resources (like images, ads, or additional scripts) are loaded later.  
🎤Example:  
✔️Critical resources: HTML, JS, CSS  
✔️Less critical: images, non-essential JS  
  
8. Tree Shaking  
🔦Purpose: Remove code that is never used in the final build.  
🔎How:  
✔️Tree-shaking is a process during bundling that eliminates dead code (unused code).  
✔️Keeps the bundle size smaller and performance higher.


## ==Redux store responsibilities

### **1. UI State (beyond what you have)**
- Modal/dialog open/close state
- Global loading spinners
- Snackbar/toast notifications
- Theme (light/dark/system)
- Sidebar/menu open/close state
- Tab or stepper navigation state

### **2. User Session & Authentication**
- User login/logout state
- Auth tokens (with care for security)
- User roles/permissions
- Session expiration and renewal

### **3. Application Settings**
- Language/locale
- Feature flags (for A/B testing or gradual rollouts)
- User preferences (e.g., default filters, sorting)

### **4. Cross-Feature Coordination**
- Global error boundaries and error reporting
- Multi-step wizards (state shared across steps)
- Undo/redo functionality
- State for drag-and-drop across lists or features

### **5. Notification & Messaging**
- In-app notifications (unread counts, message lists)
- Real-time updates (WebSocket or polling integration)
- Global banners/alerts

### **6. Caching and Persistence (for UI/app state)**
- Persisting UI state to localStorage/sessionStorage
- Restoring state on reload
- Remembering user’s last visited page or scroll position

### **7. Analytics and Tracking**
- Tracking user actions/events for analytics
- Storing temporary analytics state before sending to backend

### **8. Multi-User/Collaboration State**
- Presence/online status of other users
- Shared editing state (e.g., Google Docs-style cursors)

### **9. Advanced Workflows**
- State machines for complex flows
- Orchestrating background jobs or long-running tasks

---

### **When to Use Redux for These?**
- When the state needs to be shared across many components or features
- When you want time-travel debugging or undo/redo
- When you need to coordinate state between unrelated parts of the app
- When you want to persist state across reloads or sessions

---

### **When NOT to Use Redux for These?**
- For local component state (use `useState` or `useReducer`)
- For server state (use React Query, SWR, or similar)
- For simple, isolated state that doesn’t need to be shared

---

**Summary:**  
Redux is best for global, cross-cutting, or persistent state that is not directly tied to server data. It shines when you need predictability, debugging, and coordination across your app.