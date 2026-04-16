# JS Phase 06 — Browser APIs & DOM Mastery (Tasks 151-185)

> Framework ছাড়া DOM manipulate করতে পারলেই তুমি real JS developer। React/Vue সব DOM-ই manipulate করে — internally।

---

## Section A: DOM Fundamentals (Tasks 151-162)

### Task 151 — DOM Tree Structure 🟢

- Document Object Model:
  - HTML → parsed → DOM tree (nodes)
  - Node types: Element, Text, Comment, Document, DocumentFragment
  - `document.documentElement` (html), `document.head`, `document.body`
  - Node relationships: `parentNode`, `childNodes`, `firstChild`, `lastChild`, `nextSibling`, `previousSibling`
  - Element relationships: `parentElement`, `children`, `firstElementChild`, `lastElementChild`, `nextElementSibling`, `previousElementSibling`
  - `nodeType`, `nodeName`, `nodeValue`
  - **Build:** DOM tree visualizer — show any HTML page's DOM as tree structure

### Task 152 — Selecting Elements — All Methods 🟢

- Selection methods:
  - `document.getElementById('id')` — single element
  - `document.getElementsByClassName('class')` — live HTMLCollection
  - `document.getElementsByTagName('tag')` — live HTMLCollection
  - `document.querySelector('.class')` — first match (CSS selector)
  - `document.querySelectorAll('.class')` — static NodeList
  - `element.closest('.parent')` — find closest ancestor
  - `element.matches('.selector')` — check if matches
  - **Live vs Static:** HTMLCollection updates when DOM changes, NodeList (from querySelectorAll) doesn't
  - **Performance:** getElementById > querySelector > querySelectorAll
  - **Challenge:** Select elements 10 different ways

### Task 153 — Creating & Modifying Elements 🟡

- DOM manipulation:
  - Create: `document.createElement('div')`, `document.createTextNode('text')`
  - Insert: `parent.appendChild(child)`, `parent.insertBefore(new, ref)`
  - Modern: `parent.append(el)`, `parent.prepend(el)`, `el.before(el2)`, `el.after(el2)`, `el.replaceWith(el2)`
  - Remove: `el.remove()`, `parent.removeChild(el)`
  - Clone: `el.cloneNode(true)` (deep clone)
  - **DocumentFragment:** Batch DOM operations (avoid reflow):
    ```js
    const fragment = document.createDocumentFragment();
    for (let i = 0; i < 1000; i++) {
      const li = document.createElement('li');
      li.textContent = `Item ${i}`;
      fragment.appendChild(li);
    }
    list.appendChild(fragment); // One reflow instead of 1000
    ```
  - **Build:** Dynamic table generator (rows from data array)

### Task 154 — Attributes & Properties 🟡

- Attribute vs Property:
  - Attributes: HTML attributes (`el.getAttribute()`, `el.setAttribute()`)
  - Properties: DOM object properties (`el.id`, `el.className`)
  - Usually synced but not always:
    - `input.getAttribute('value')` = initial value
    - `input.value` = current value
  - `dataset` — custom data attributes:
    ```html
    <div data-user-id="123" data-role="admin"></div>
    ```
    ```js
    el.dataset.userId; // "123"
    el.dataset.role; // "admin"
    ```
  - **Build:** Data-attribute-driven component system

### Task 155 — CSS Manipulation 🟡

- Styling with JS:
  - `el.style.property = value` (inline styles)
  - `el.style.cssText = 'color: red; font-size: 16px;'`
  - `el.classList.add()`, `.remove()`, `.toggle()`, `.contains()`, `.replace()`
  - `getComputedStyle(el)` — final computed styles
  - CSS Custom Properties: `el.style.setProperty('--color', 'red')`
  - `getComputedStyle(el).getPropertyValue('--color')`
  - **Build:** Theme switcher (CSS variables + JS toggle)
  - **Build:** Dynamic style generator (create <style> elements)

### Task 156 — innerHTML vs textContent vs innerText 🟢

- Content methods:
  - `innerHTML` — HTML string (parses HTML)
  - `textContent` — all text (including hidden) — no HTML parsing
  - `innerText` — visible text only (triggers reflow)
  - XSS danger with `innerHTML`:

    ```js
    // DANGEROUS:
    el.innerHTML = userInput; // If userInput has <script> tags!

    // SAFE:
    el.textContent = userInput; // Treated as plain text
    ```

  - `insertAdjacentHTML('position', html)` — safer insertion
  - **Build:** XSS-safe template engine using `textContent` + `createElement`

### Task 157 — Event System Deep Dive 🔴

- DOM Events:
  - Event phases: **Capturing** → **Target** → **Bubbling**
  - `addEventListener(type, handler, options)`:
    - `options.capture` — listen during capture phase
    - `options.once` — auto-remove after first call
    - `options.passive` — won't call `preventDefault()`
    - `options.signal` — AbortController for removal
  - `removeEventListener(type, handler)`
  - **Event object properties:**
    - `e.target` — element that triggered event
    - `e.currentTarget` — element that handler is on
    - `e.type`, `e.timeStamp`
    - `e.preventDefault()` — prevent default behavior
    - `e.stopPropagation()` — stop bubbling
    - `e.stopImmediatePropagation()` — stop ALL handlers
  - **Build:** Event flow visualizer (show capture → target → bubble)

### Task 158 — Event Delegation 🟡

- Handle events on parent:
  ```js
  // Instead of adding listener to each button:
  document.querySelector('.button-container').addEventListener('click', (e) => {
    if (e.target.matches('.btn-delete')) handleDelete(e.target.dataset.id);
    if (e.target.matches('.btn-edit')) handleEdit(e.target.dataset.id);
    if (e.target.closest('.btn-view')) handleView(e.target.closest('.btn-view').dataset.id);
  });
  ```

  - Benefits: memory efficient, handles dynamic elements
  - `e.target.matches()` ও `e.target.closest()` for delegation
  - **Build:** Todo app using ONLY event delegation (no direct listeners on items)
  - **Build:** Dynamic table with sort/filter/delete using delegation

### Task 159 — Custom Events 🟡

- Create custom events:

  ```js
  // Create
  const event = new CustomEvent('user:login', {
    detail: { userId: 123, name: 'John' },
    bubbles: true,
    cancelable: true,
  });

  // Dispatch
  element.dispatchEvent(event);

  // Listen
  element.addEventListener('user:login', (e) => {
    console.log(e.detail.userId);
  });
  ```

  - **Build:** Component communication system using custom events
  - **Build:** Undo system using custom events

### Task 160 — Keyboard & Mouse Events 🟡

- Input events:
  - Keyboard: `keydown`, `keyup`, `keypress` (deprecated)
  - `e.key` (character), `e.code` (physical key), `e.altKey`, `e.ctrlKey`, `e.shiftKey`, `e.metaKey`
  - Mouse: `click`, `dblclick`, `mousedown`, `mouseup`, `mousemove`, `mouseenter`, `mouseleave`, `mouseover`, `mouseout`, `contextmenu`
  - `e.clientX/Y` (viewport), `e.pageX/Y` (page), `e.offsetX/Y` (element), `e.screenX/Y` (screen)
  - Touch: `touchstart`, `touchmove`, `touchend`, `touchcancel`
  - Pointer: `pointerdown`, `pointermove`, `pointerup` (unified mouse+touch)
  - **Build:** Drawing canvas (mouse/touch support)
  - **Build:** Keyboard shortcut system (Ctrl+S, Ctrl+Z, etc.)

### Task 161 — Form Events & Validation 🟡

- Form handling:
  - Events: `submit`, `reset`, `input`, `change`, `focus`, `blur`, `invalid`
  - `FormData` API:
    ```js
    form.addEventListener('submit', (e) => {
      e.preventDefault();
      const formData = new FormData(e.target);
      const data = Object.fromEntries(formData);
    });
    ```
  - Constraint Validation API:
    - `input.validity` — ValidityState object
    - `input.checkValidity()` — returns boolean
    - `input.reportValidity()` — shows browser message
    - `input.setCustomValidity('message')` — custom error
  - **Build:** Multi-step form with custom validation (no libraries)

### Task 162 — Scroll & Resize Events (Performance) 🟡

- Performance-sensitive events:
  - `scroll`, `resize` — fire very frequently
  - `IntersectionObserver` — observe element visibility:

    ```js
    const observer = new IntersectionObserver(
      (entries) => {
        entries.forEach((entry) => {
          if (entry.isIntersecting) {
            entry.target.classList.add('visible');
            observer.unobserve(entry.target); // stop watching
          }
        });
      },
      { threshold: 0.1 }
    );

    document.querySelectorAll('.lazy').forEach((el) => observer.observe(el));
    ```

  - `ResizeObserver` — observe element size changes
  - `MutationObserver` — observe DOM changes
  - **Build:** Infinite scroll with IntersectionObserver
  - **Build:** Lazy image loader with IntersectionObserver

---

## Section B: Web APIs (Tasks 163-175)

### Task 163 — Storage APIs 🟡

- Client-side storage:
  - **localStorage:** persistent, 5-10MB, sync, string only
  - **sessionStorage:** per tab, cleared on close
  - **Cookies:** 4KB, sent with requests, expiry
    ```js
    document.cookie = 'name=value; expires=date; path=/; secure; samesite=strict';
    ```
  - **IndexedDB:** large data, async, structured data
  - **Cache API:** HTTP response caching (Service Workers)
  - **Build:** Storage abstraction layer:
    ```js
    const storage = createStorage('localStorage'); // or 'indexedDB', 'sessionStorage'
    await storage.set('key', value);
    await storage.get('key');
    await storage.delete('key');
    ```
  - **Build:** Shopping cart with localStorage persistence

### Task 164 — IndexedDB Mastery 🔴

- Browser database:

  ```js
  const request = indexedDB.open('MyDB', 1);

  request.onupgradeneeded = (e) => {
    const db = e.target.result;
    const store = db.createObjectStore('users', { keyPath: 'id', autoIncrement: true });
    store.createIndex('email', 'email', { unique: true });
  };
  ```

  - CRUD operations with transactions
  - Indexes for querying
  - Cursors for iteration
  - Versioning ও migrations
  - **Build:** Full CRUD app using IndexedDB:
    - Notes app with tags, search, sort
    - Offline-first with sync indicator
  - **Build:** IndexedDB wrapper library (Promise-based)

### Task 165 — Fetch API Complete 🟡

- Beyond basic fetch:
  - Request options: method, headers, body, mode, credentials, cache, redirect, signal
  - Response: status, ok, headers, body (ReadableStream)
  - Upload: `FormData` for files
  - Streaming: response body as stream
  - CORS: how it works, preflight requests
  - **Build:** Complete API client:
    ```js
    const api = createClient({ baseURL: '/api', headers: { Authorization: `Bearer ${token}` } });
    const users = await api.get('/users');
    await api.post('/users', { name: 'John' });
    await api.put('/users/1', { name: 'Jane' });
    await api.delete('/users/1');
    ```

### Task 166 — URL & URLSearchParams API 🟢

- URL manipulation:

  ```js
  const url = new URL('https://example.com/path?q=search#hash');
  url.hostname; // "example.com"
  url.pathname; // "/path"
  url.searchParams.get('q'); // "search"
  url.hash; // "#hash"

  const params = new URLSearchParams({ page: 1, sort: 'name' });
  params.append('filter', 'active');
  params.toString(); // "page=1&sort=name&filter=active"
  ```

  - **Build:** URL router (SPA routing without framework)

### Task 167 — History API & SPA Routing 🟡

- Browser history manipulation:
  - `history.pushState(state, '', url)` — add to history
  - `history.replaceState(state, '', url)` — replace current
  - `window.addEventListener('popstate', handler)` — back/forward
  - **Build:** Complete SPA router:
    ```js
    const router = createRouter({
      '/': HomePage,
      '/about': AboutPage,
      '/users/:id': UserPage,
      '/404': NotFoundPage,
    });
    router.navigate('/users/123');
    ```
  - Features: path parameters, query params, guards, lazy loading

### Task 168 — Canvas API 🟡

- 2D drawing:

  ```js
  const canvas = document.getElementById('canvas');
  const ctx = canvas.getContext('2d');

  ctx.fillStyle = 'red';
  ctx.fillRect(10, 10, 100, 50);
  ctx.strokeRect(10, 10, 100, 50);
  ctx.clearRect(20, 20, 50, 25);

  // Path
  ctx.beginPath();
  ctx.arc(200, 200, 50, 0, Math.PI * 2);
  ctx.fill();

  // Text
  ctx.font = '24px Arial';
  ctx.fillText('Hello Canvas', 10, 200);

  // Image
  const img = new Image();
  img.onload = () => ctx.drawImage(img, 0, 0);
  img.src = 'image.png';
  ```

  - **Build:** Drawing app (pen, shapes, colors, eraser, undo)
  - **Build:** Simple game (snake, pong, or breakout)

### Task 169 — Web Animations API 🟡

- Animate without CSS:
  ```js
  element.animate([{ transform: 'translateX(0)' }, { transform: 'translateX(300px)' }], {
    duration: 1000,
    easing: 'ease-in-out',
    fill: 'forwards',
    iterations: Infinity,
  });
  ```

  - `requestAnimationFrame` for custom animations:
    ```js
    function animate(timestamp) {
      // Update positions
      // Draw frame
      requestAnimationFrame(animate);
    }
    requestAnimationFrame(animate);
    ```
  - **Build:** Animation library (fade, slide, bounce, custom easing)
  - **Build:** Particle system using Canvas + rAF

### Task 170 — Geolocation API 🟢

- Get user location:

  ```js
  navigator.geolocation.getCurrentPosition(
    (pos) => console.log(pos.coords.latitude, pos.coords.longitude),
    (err) => console.error(err),
    { enableHighAccuracy: true, timeout: 5000, maximumAge: 0 }
  );

  const watchId = navigator.geolocation.watchPosition(callback);
  navigator.geolocation.clearWatch(watchId);
  ```

  - **Build:** Location-based app (show user on map, calculate distances)

### Task 171 — Notification API & Permissions 🟢

- Browser notifications:
  ```js
  const permission = await Notification.requestPermission();
  if (permission === 'granted') {
    new Notification('Hello!', {
      body: 'This is a notification',
      icon: '/icon.png',
      tag: 'unique-tag', // prevents duplicates
    });
  }
  ```

  - Permissions API: `navigator.permissions.query({ name: 'notifications' })`
  - **Build:** Notification manager with queue ও priorities

### Task 172 — Clipboard API 🟢

- Copy/paste programmatically:

  ```js
  // Modern async API
  await navigator.clipboard.writeText('Copied text');
  const text = await navigator.clipboard.readText();

  // Rich content
  const blob = new Blob(['<b>Bold</b>'], { type: 'text/html' });
  await navigator.clipboard.write([new ClipboardItem({ 'text/html': blob })]);
  ```

  - **Build:** Code snippet copier with "Copied!" feedback

### Task 173 — Drag & Drop API 🟡

- Native drag and drop:
  - Draggable: `draggable="true"`, events: `dragstart`, `drag`, `dragend`
  - Drop zone: events: `dragenter`, `dragover`, `dragleave`, `drop`
  - `e.dataTransfer.setData()`, `e.dataTransfer.getData()`
  - File drop: `e.dataTransfer.files`
  - **Build:** Kanban board with drag & drop (pure JS)
  - **Build:** File upload with drag & drop zone

### Task 174 — File & Blob APIs 🟡

- File handling:
  - `File` extends `Blob`
  - `FileReader`: readAsText, readAsDataURL, readAsArrayBuffer
  - `URL.createObjectURL(blob)` — create URL for blob
  - `URL.revokeObjectURL(url)` — cleanup
  - File System Access API (modern):
    ```js
    const [handle] = await window.showOpenFilePicker();
    const file = await handle.getFile();
    const content = await file.text();
    ```
  - **Build:** File manager (open, read, display, save files)
  - **Build:** Image preview before upload

### Task 175 — Web Audio API 🟡

- Audio processing:

  ```js
  const ctx = new AudioContext();
  const oscillator = ctx.createOscillator();
  const gainNode = ctx.createGain();

  oscillator.type = 'sine'; // sine, square, sawtooth, triangle
  oscillator.frequency.value = 440; // A4
  gainNode.gain.value = 0.5;

  oscillator.connect(gainNode).connect(ctx.destination);
  oscillator.start();
  ```

  - **Build:** Piano keyboard (playable with mouse ও keyboard)
  - **Build:** Audio visualizer (frequency bars with Canvas)

---

## Section C: Advanced Browser APIs (Tasks 176-185)

### Task 176 — Service Workers Basics 🔴

- Offline-capable apps:

  ```js
  // Register
  navigator.serviceWorker.register('/sw.js');

  // sw.js
  self.addEventListener('install', (e) => {
    e.waitUntil(caches.open('v1').then((cache) => cache.addAll(['/'])));
  });

  self.addEventListener('fetch', (e) => {
    e.respondWith(caches.match(e.request).then((cached) => cached || fetch(e.request)));
  });
  ```

  - Lifecycle: install → waiting → activate
  - Caching strategies: cache-first, network-first, stale-while-revalidate
  - **Build:** Offline-capable news reader

### Task 177 — Web Components 🔴

- Custom elements:

  ```js
  class MyCard extends HTMLElement {
    constructor() {
      super();
      this.attachShadow({ mode: 'open' });
    }

    connectedCallback() {
      this.shadowRoot.innerHTML = `
        <style>:host { display: block; padding: 16px; }</style>
        <slot name="title"></slot>
        <slot></slot>
      `;
    }

    static get observedAttributes() {
      return ['title'];
    }
    attributeChangedCallback(name, oldVal, newVal) {
      /* ... */
    }
  }

  customElements.define('my-card', MyCard);
  ```

  - Shadow DOM: encapsulated styles ও DOM
  - `<template>` ও `<slot>`
  - **Build:** 5 reusable web components: modal, tabs, accordion, tooltip, dropdown

### Task 178 — Performance APIs 🟡

- Measure performance:
  - `performance.now()` — high-resolution timestamp
  - `performance.mark()` ও `performance.measure()`:
    ```js
    performance.mark('start');
    doExpensiveWork();
    performance.mark('end');
    performance.measure('work', 'start', 'end');
    const measure = performance.getEntriesByName('work')[0];
    console.log(`Took ${measure.duration}ms`);
    ```
  - `PerformanceObserver` — observe performance entries
  - Navigation Timing: page load metrics
  - Resource Timing: individual resource load times
  - **Build:** Performance monitoring dashboard

### Task 179 — Page Visibility & Lifecycle 🟢

- Detect tab visibility:
  ```js
  document.addEventListener('visibilitychange', () => {
    if (document.hidden) {
      // Pause video, reduce updates
    } else {
      // Resume
    }
  });
  ```

  - `document.visibilityState`: 'visible', 'hidden'
  - Page lifecycle: `beforeunload`, `unload`, `pagehide`, `pageshow`
  - `navigator.sendBeacon(url, data)` — guaranteed delivery before unload
  - **Build:** App that pauses when tab hidden (save resources)

### Task 180 — Broadcast Channel & postMessage 🟡

- Cross-tab/window communication:

  ```js
  // Tab 1
  const channel = new BroadcastChannel('my-app');
  channel.postMessage({ type: 'LOGOUT' });

  // Tab 2
  const channel = new BroadcastChannel('my-app');
  channel.onmessage = (e) => {
    if (e.data.type === 'LOGOUT') window.location = '/login';
  };
  ```

  - `window.postMessage()` — cross-origin communication (iframes)
  - **Build:** Sync shopping cart across tabs
  - **Build:** Cross-tab auth (logout all tabs when one logs out)

### Task 181 — Web Crypto API 🟡

- Client-side cryptography:

  ```js
  // Hash
  const hash = await crypto.subtle.digest('SHA-256', data);

  // Encrypt/Decrypt
  const key = await crypto.subtle.generateKey({ name: 'AES-GCM', length: 256 }, true, [
    'encrypt',
    'decrypt',
  ]);
  const encrypted = await crypto.subtle.encrypt({ name: 'AES-GCM', iv }, key, data);

  // Random
  crypto.getRandomValues(new Uint8Array(16));
  crypto.randomUUID(); // UUID v4
  ```

  - **Build:** End-to-end encrypted note app (encrypt before storage)

### Task 182 — requestIdleCallback & Scheduling 🟡

- Schedule non-critical work:
  ```js
  requestIdleCallback((deadline) => {
    while (deadline.timeRemaining() > 0 && tasks.length > 0) {
      processTask(tasks.pop());
    }
    if (tasks.length > 0) requestIdleCallback(processRemainingTasks);
  });
  ```

  - `requestAnimationFrame` — before repaint (visual updates)
  - `requestIdleCallback` — when browser is idle (non-critical)
  - `scheduler.postTask()` — priority-based scheduling (new API)
  - **Build:** Background data processor that doesn't block UI

### Task 183 — Screen, Battery, Network APIs 🟢

- Device APIs:

  ```js
  // Screen
  (screen.width, screen.height, screen.orientation);
  window.matchMedia('(prefers-color-scheme: dark)').matches;

  // Battery
  const battery = await navigator.getBattery();
  (battery.level, battery.charging);

  // Network
  navigator.onLine; // boolean
  navigator.connection.effectiveType; // "4g", "3g", "2g", "slow-2g"

  window.addEventListener('online', handler);
  window.addEventListener('offline', handler);
  ```

  - **Build:** Adaptive app (reduce quality on slow network, save battery mode)

### Task 184 — Build: Mini jQuery (DOM Library) ⚫

- jQuery-lite (pure JS):

  ```js
  const $ = (selector) => new DOMLib(selector);

  // Usage:
  $('.btn').addClass('active').css('color', 'red').on('click', handler).text('Click me');

  // Features:
  // Selection, traversal, manipulation, events, CSS, animation, AJAX
  ```

  - Methods: `find`, `parent`, `children`, `siblings`, `closest`, `addClass`, `removeClass`, `toggleClass`, `hasClass`, `attr`, `data`, `css`, `text`, `html`, `val`, `append`, `prepend`, `remove`, `on`, `off`, `trigger`, `fadeIn`, `fadeOut`, `slideDown`, `slideUp`, `animate`, `ajax`
  - **Output:** Complete DOM manipulation library (~300 lines)

### Task 185 — Build: SPA Framework (Mini React) ⚫

- Build a tiny reactive UI framework:

  ```js
  // Virtual DOM
  function createElement(type, props, ...children) {
    return { type, props: props || {}, children };
  }

  // Diff & Patch
  function diff(oldTree, newTree) {
    /* ... */
  }
  function patch(parent, patches) {
    /* ... */
  }

  // Reactive State
  function useState(initial) {
    // Returns [state, setState]
    // setState triggers re-render
  }

  // Component
  function Counter() {
    const [count, setCount] = useState(0);
    return createElement(
      'div',
      null,
      createElement('p', null, `Count: ${count}`),
      createElement('button', { onclick: () => setCount(count + 1) }, '+')
    );
  }

  render(Counter, document.getElementById('root'));
  ```

  - Features: virtual DOM, diffing, reactive state, event handling, component lifecycle
  - **Output:** Working mini-framework that renders components

---

## ✅ Phase 06 Completion Checklist

- [ ] DOM tree structure ও node types বোঝো
- [ ] Elements select, create, modify, remove efficiently পারো
- [ ] Event system (capture, bubble, delegation) পারো
- [ ] Custom events create ও dispatch করতে পারো
- [ ] All storage APIs জানো (localStorage, IndexedDB, Cache)
- [ ] Fetch API complete ব্যবহার করতে পারো
- [ ] Canvas API দিয়ে draw করতে পারো
- [ ] History API দিয়ে SPA routing করতে পারো
- [ ] Web Components build করতে পারো
- [ ] Service Workers ও offline capability বোঝো
- [ ] Performance APIs ব্যবহার করতে পারো
- [ ] Mini jQuery ও Mini React build করতে পারো

> ✅ Ready? → **JS Phase 07: Node.js & Server-Side JS**
