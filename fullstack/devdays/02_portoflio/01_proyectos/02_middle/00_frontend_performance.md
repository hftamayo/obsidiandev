
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