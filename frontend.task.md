# 🚀 50 Frontend Tasks — UI, Functionality, React, Next.js & Third-Party Libraries

> প্রতিটা task নিজে নিজে করো। copy-paste করলে skill improve হবে না।
> Difficulty: 🟢 Easy | 🟡 Medium | 🔴 Hard

---

## 📌 UI & CSS Tasks

### Task 1 — Responsive Portfolio Website 🟢

- **Goal:** নিজের একটা portfolio website বানাও
- **Requirements:**
  - Mobile-first responsive design (Flexbox + Grid)
  - Hero section, About, Projects, Contact section
  - Smooth scroll navigation
  - CSS animations on scroll (AOS library ব্যবহার করো)
  - Dark/Light mode toggle
- **Tech:** HTML, CSS, React
- **Third-Party:** `aos` (Animate On Scroll)

---

### Task 2 — Glassmorphism Dashboard UI 🟡

- **Goal:** একটা admin dashboard এর UI বানাও glassmorphism design এ
- **Requirements:**
  - Sidebar navigation with icons
  - Stats cards with glassmorphism effect
  - Charts section (placeholder)
  - Responsive layout for tablet & mobile
  - CSS backdrop-filter, box-shadow ব্যবহার করো
- **Tech:** React, CSS Modules
- **Third-Party:** `react-icons`

---

### Task 3 — Animated Landing Page 🟡

- **Goal:** একটা SaaS product এর landing page বানাও with micro-animations
- **Requirements:**
  - Framer Motion দিয়ে entrance animations
  - Parallax scrolling effect
  - Pricing cards with hover effects
  - Testimonial carousel/slider
  - Fully responsive
- **Tech:** React, Framer Motion
- **Third-Party:** `framer-motion`, `swiper`

---

### Task 4 — CSS Grid Image Gallery 🟢

- **Goal:** Pinterest-style masonry image gallery
- **Requirements:**
  - CSS Grid দিয়ে masonry layout
  - Image hover effect (zoom + overlay with title)
  - Lightbox modal on click
  - Filter by category (All, Nature, City, People)
  - Lazy loading images
- **Tech:** React
- **Third-Party:** `react-photo-album`, `yet-another-react-lightbox`

---

### Task 5 — Neumorphism Calculator 🟢

- **Goal:** Neumorphic design এ একটা calculator বানাও
- **Requirements:**
  - Soft UI / Neumorphism design
  - Basic operations (+, -, ×, ÷)
  - Keyboard support
  - History of calculations
  - Button press animation
- **Tech:** React, CSS

---

## 📌 React Functionality Tasks

### Task 6 — Advanced Todo App with Drag & Drop 🟡

- **Goal:** Todo app with drag-and-drop reordering & categories
- **Requirements:**
  - Add, edit, delete todos
  - Drag & drop দিয়ে reorder করা যাবে
  - Categories (Work, Personal, Shopping)
  - Filter by status (All, Active, Completed)
  - LocalStorage এ data persist করো
- **Tech:** React, Context API
- **Third-Party:** `@dnd-kit/core`, `@dnd-kit/sortable`

---

### Task 7 — Real-time Search with Debounce 🟡

- **Goal:** একটা search page বানাও যেটা API থেকে data fetch করবে with debounce
- **Requirements:**
  - Input field এ type করলে debounce এর পর API call হবে
  - Loading skeleton while fetching
  - Search results highlight matched text
  - Recent searches save করো (localStorage)
  - No results state handle করো
- **Tech:** React, Custom Hooks
- **API:** JSONPlaceholder বা Open Library API

---

### Task 8 — Multi-Step Form with Validation 🟡

- **Goal:** 4-step registration form with progress bar
- **Requirements:**
  - Step 1: Personal Info (name, email, phone)
  - Step 2: Address Info
  - Step 3: Account Setup (password, confirm)
  - Step 4: Review & Submit
  - Each step এ validation (React Hook Form + Zod)
  - Progress bar showing current step
- **Tech:** React
- **Third-Party:** `react-hook-form`, `zod`, `@hookform/resolvers`

---

### Task 9 — Infinite Scroll News Feed 🟡

- **Goal:** Social media style news feed with infinite scroll
- **Requirements:**
  - Scroll করলে automatically নতুন posts load হবে
  - Intersection Observer API ব্যবহার করো
  - Post card with image, title, description, like button
  - Skeleton loader while loading
  - "Back to top" button
- **Tech:** React, Custom Hooks
- **API:** NewsAPI বা JSONPlaceholder

---

### Task 10 — Shopping Cart System 🟡

- **Goal:** E-commerce style shopping cart
- **Requirements:**
  - Product listing page with grid layout
  - Add to cart functionality
  - Cart page with quantity update (+/-)
  - Remove item from cart
  - Total price calculation with discount
  - useReducer দিয়ে cart state manage করো
- **Tech:** React, useReducer, Context API

---

### Task 11 — Authentication Flow UI 🟡

- **Goal:** Complete auth flow — Login, Register, Forgot Password
- **Requirements:**
  - Login form with email/password
  - Register form with validation
  - Forgot password flow (email input → OTP → new password)
  - Show/hide password toggle
  - Form validation with error messages
  - Protected route simulation (redirect if not logged in)
- **Tech:** React, React Router
- **Third-Party:** `react-router-dom`, `react-hook-form`

---

### Task 12 — Custom Hook Collection 🟡

- **Goal:** 5টা useful custom hooks বানাও এবং demo page বানাও
- **Requirements:**
  - `useLocalStorage` — localStorage sync
  - `useDebounce` — debounced value
  - `useFetch` — data fetching with loading/error
  - `useClickOutside` — detect outside click
  - `useMediaQuery` — responsive breakpoint detect
  - প্রতিটা hook এর interactive demo
- **Tech:** React

---

### Task 13 — Data Table with Sort, Filter & Pagination 🔴

- **Goal:** Feature-rich data table component
- **Requirements:**
  - Column-wise sorting (ascending/descending)
  - Global search filter
  - Column-specific filters
  - Pagination with page size selector
  - Row selection with checkbox
  - Export to CSV
- **Tech:** React
- **Third-Party:** `@tanstack/react-table`

---

### Task 14 — Kanban Board 🔴

- **Goal:** Trello-style kanban board
- **Requirements:**
  - 3 columns: Todo, In Progress, Done
  - Cards drag & drop between columns
  - Add new card with title & description
  - Edit & delete cards
  - Color labels/tags
  - LocalStorage persistence
- **Tech:** React
- **Third-Party:** `@dnd-kit/core`, `@dnd-kit/sortable`

---

### Task 15 — Real-time Chat UI 🟡

- **Goal:** WhatsApp/Messenger style chat interface
- **Requirements:**
  - Chat list sidebar with search
  - Chat window with message bubbles
  - Sent vs received message styling
  - Typing indicator animation
  - Timestamp on messages
  - Emoji picker
  - Auto-scroll to latest message
- **Tech:** React
- **Third-Party:** `emoji-picker-react`

---

## 📌 React + Third-Party Library Tasks

### Task 16 — Interactive Charts Dashboard 🟡

- **Goal:** Analytics dashboard with multiple chart types
- **Requirements:**
  - Line chart (monthly revenue)
  - Bar chart (product sales)
  - Pie/Doughnut chart (category distribution)
  - Area chart (user growth)
  - Date range filter to update charts
  - Responsive charts
- **Tech:** React
- **Third-Party:** `recharts` অথবা `chart.js` + `react-chartjs-2`

---

### Task 17 — Map-Based Location Finder 🟡

- **Goal:** Interactive map with location search & markers
- **Requirements:**
  - Map display with zoom controls
  - Search location by name
  - Click on map to add marker
  - Show current user location
  - Popup on marker click with info
  - List of saved locations
- **Tech:** React
- **Third-Party:** `react-leaflet`, `leaflet`

---

### Task 18 — Rich Text Editor / Blog Writer 🔴

- **Goal:** Medium-style blog writing interface
- **Requirements:**
  - Rich text editing (bold, italic, headings, lists)
  - Image upload/embed
  - Code block support
  - Preview mode (side by side)
  - Auto-save draft (localStorage)
  - Word count
- **Tech:** React
- **Third-Party:** `tiptap` অথবা `react-quill`

---

### Task 19 — File Upload with Preview & Progress 🟡

- **Goal:** Drag & drop file uploader
- **Requirements:**
  - Drag & drop zone
  - Click to browse files
  - Image preview before upload
  - Upload progress bar
  - File type & size validation
  - Multiple file support
  - Remove file from queue
- **Tech:** React
- **Third-Party:** `react-dropzone`

---

### Task 20 — PDF Invoice Generator 🔴

- **Goal:** Invoice create করো এবং PDF download করো
- **Requirements:**
  - Form: company info, client info, items list
  - Dynamic item rows (add/remove)
  - Auto-calculate subtotal, tax, total
  - Invoice preview
  - Download as PDF
  - Invoice number auto-generate
- **Tech:** React
- **Third-Party:** `@react-pdf/renderer` অথবা `jspdf` + `html2canvas`

---

### Task 21 — Calendar & Event Scheduler 🔴

- **Goal:** Google Calendar style event manager
- **Requirements:**
  - Monthly/Weekly/Daily view
  - Click on date to add event
  - Event modal with title, time, description, color
  - Drag to reschedule events
  - Event filtering
  - LocalStorage persistence
- **Tech:** React
- **Third-Party:** `react-big-calendar`, `date-fns`

---

### Task 22 — Notification System 🟡

- **Goal:** Toast notification system like production apps
- **Requirements:**
  - Success, Error, Warning, Info toasts
  - Auto-dismiss with timer
  - Manual dismiss
  - Stack multiple notifications
  - Position options (top-right, bottom-left, etc.)
  - Custom notification with action buttons
  - Build your own + compare with library
- **Tech:** React
- **Third-Party:** `react-hot-toast` অথবা `sonner`

---

### Task 23 — Image Cropper & Editor 🟡

- **Goal:** Profile picture upload with crop functionality
- **Requirements:**
  - Upload image
  - Crop with adjustable area
  - Aspect ratio lock (1:1 for profile, 16:9 for cover)
  - Zoom & rotate
  - Preview cropped result
  - Download cropped image
- **Tech:** React
- **Third-Party:** `react-cropper` অথবা `react-image-crop`

---

### Task 24 — Animated Page Transitions 🟡

- **Goal:** Multi-page React app with smooth page transitions
- **Requirements:**
  - 5+ pages (Home, About, Projects, Blog, Contact)
  - Framer Motion দিয়ে page transition animations
  - Different animation per route (fade, slide, scale)
  - Shared layout animations
  - Navigation with active state indicator
- **Tech:** React, React Router
- **Third-Party:** `framer-motion`, `react-router-dom`

---

### Task 25 — Music Player UI 🟡

- **Goal:** Spotify-style music player
- **Requirements:**
  - Playlist sidebar
  - Now playing bar at bottom
  - Play, Pause, Next, Previous controls
  - Progress bar with seek
  - Volume control
  - Shuffle & Repeat toggle
  - Album art display
  - Use HTML5 Audio API
- **Tech:** React, useRef
- **Third-Party:** `react-icons`, `react-h5-audio-player` (optional)

---

## 📌 Next.js Tasks

### Task 26 — Blog with SSG (Static Site Generation) 🟡

- **Goal:** Markdown-based blog with Next.js SSG
- **Requirements:**
  - Markdown files থেকে blog posts generate করো
  - Home page এ all posts list (statically generated)
  - Individual post page with `generateStaticParams`
  - Reading time calculation
  - Table of contents auto-generate
  - SEO meta tags (Next.js Metadata API)
- **Tech:** Next.js (App Router)
- **Third-Party:** `gray-matter`, `remark`, `remark-html`

---

### Task 27 — Server-Side Rendered (SSR) Product Page 🟡

- **Goal:** E-commerce product page with SSR
- **Requirements:**
  - Product data server-side fetch করো
  - Dynamic OG image generation
  - Product image gallery
  - Size/Color selector
  - Related products section
  - SEO optimized with proper meta tags
- **Tech:** Next.js (App Router), Server Components
- **API:** Fake Store API

---

### Task 28 — Next.js Authentication with NextAuth 🔴

- **Goal:** Complete authentication system
- **Requirements:**
  - Google OAuth login
  - GitHub OAuth login
  - Email/Password login (Credentials provider)
  - Protected routes (middleware)
  - User profile page
  - Session management
  - Sign out functionality
- **Tech:** Next.js (App Router)
- **Third-Party:** `next-auth`

---

### Task 29 — API Routes & Full-Stack CRUD 🟡

- **Goal:** Notes app with Next.js API routes
- **Requirements:**
  - Next.js Route Handlers (GET, POST, PUT, DELETE)
  - Notes list page (Server Component)
  - Create/Edit note form (Client Component)
  - Delete with confirmation
  - Search & filter notes
  - JSON file বা SQLite দিয়ে data store করো
- **Tech:** Next.js (App Router)
- **Third-Party:** `better-sqlite3` অথবা just JSON file

---

### Task 30 — ISR (Incremental Static Regeneration) News Site 🟡

- **Goal:** News website with ISR
- **Requirements:**
  - Home page revalidates every 60 seconds
  - Category pages (Sports, Tech, Business)
  - Individual article page with ISR
  - Breaking news banner (client-side real-time)
  - Share buttons
  - Reading time estimate
- **Tech:** Next.js (App Router), ISR
- **API:** NewsAPI অথবা mock data

---

### Task 31 — Next.js Middleware & Edge Functions 🔴

- **Goal:** Advanced middleware use cases
- **Requirements:**
  - Geo-based content (detect country, show relevant content)
  - A/B testing via middleware (different UI for different users)
  - Rate limiting on API routes
  - Auth redirect (not logged in → login page)
  - Request logging middleware
  - Custom 404 & error pages
- **Tech:** Next.js (App Router), Middleware

---

### Task 32 — Parallel & Intercepting Routes 🔴

- **Goal:** Instagram-style photo gallery with Next.js advanced routing
- **Requirements:**
  - Photo grid on main page
  - Click photo → modal overlay (intercepting route)
  - Direct URL to photo → full page view (parallel route)
  - User profile sidebar always visible
  - Loading states for each route segment
  - Error boundaries per segment
- **Tech:** Next.js (App Router), Parallel Routes, Intercepting Routes

---

### Task 33 — Server Actions Form Handling 🟡

- **Goal:** Contact form & feedback system with Server Actions
- **Requirements:**
  - Contact form with Server Action
  - Form validation (server-side with Zod)
  - useFormStatus for loading state
  - useActionState for form state management
  - Success/Error toast messages
  - Optimistic UI updates
- **Tech:** Next.js (App Router), Server Actions
- **Third-Party:** `zod`, `sonner`

---

### Task 34 — Dynamic OG Image Generation 🟡

- **Goal:** Blog posts এর জন্য dynamic Open Graph images
- **Requirements:**
  - Next.js `ImageResponse` API ব্যবহার করো
  - Blog title, author, date OG image তে show করো
  - Custom fonts in OG image
  - Different templates for different post types
  - Twitter card support
  - Preview OG images in a debug page
- **Tech:** Next.js (App Router), `next/og`

---

### Task 35 — Streaming & Suspense Dashboard 🔴

- **Goal:** Analytics dashboard with streaming SSR
- **Requirements:**
  - Multiple data sections that load independently
  - React Suspense boundaries for each section
  - Streaming server-side rendering
  - Loading skeletons per section
  - Error boundaries per section
  - Progressive page load (fast sections show first)
- **Tech:** Next.js (App Router), Suspense, Streaming

---

## 📌 Advanced Combination Tasks (React + Next.js + Libraries)

### Task 36 — E-commerce Store (Full Frontend) 🔴

- **Goal:** Complete e-commerce frontend
- **Requirements:**
  - Product listing with filters (price, category, rating)
  - Product detail page with image zoom
  - Shopping cart (Zustand state management)
  - Wishlist functionality
  - Checkout form (multi-step)
  - Order confirmation page
  - Responsive design
- **Tech:** Next.js (App Router)
- **Third-Party:** `zustand`, `swiper`, `react-hook-form`, `zod`

---

### Task 37 — Social Media Dashboard 🔴

- **Goal:** Twitter/X style social media frontend
- **Requirements:**
  - Feed with posts (text, images)
  - Create post with character limit
  - Like, Comment, Repost functionality
  - User profile page
  - Follow/Unfollow UI
  - Trending topics sidebar
  - Infinite scroll feed
  - Real-time-like notifications
- **Tech:** Next.js, React
- **Third-Party:** `zustand`, `framer-motion`, `date-fns`

---

### Task 38 — Project Management Tool 🔴

- **Goal:** Jira/Asana style project management UI
- **Requirements:**
  - Project list with status
  - Kanban board view per project
  - List view with sorting & filtering
  - Task detail modal with comments
  - Team member assignment
  - Due date with calendar picker
  - Priority labels (High, Medium, Low)
  - Dashboard with project stats charts
- **Tech:** Next.js
- **Third-Party:** `@dnd-kit/core`, `recharts`, `date-fns`, `react-hook-form`

---

### Task 39 — Real-time Collaborative Whiteboard 🔴

- **Goal:** Figma/Miro style drawing board
- **Requirements:**
  - Canvas drawing with different tools (pen, shapes, text)
  - Color picker & stroke width
  - Undo/Redo functionality
  - Zoom & pan
  - Export as PNG
  - Layer management
  - Use HTML Canvas API
- **Tech:** React, Canvas API
- **Third-Party:** `rough.js` (optional, for hand-drawn style)

---

### Task 40 — Video Streaming Platform UI 🔴

- **Goal:** YouTube-style video platform frontend
- **Requirements:**
  - Home page with video grid (thumbnails)
  - Video player page with custom controls
  - Recommended videos sidebar
  - Comments section with replies
  - Like/Dislike system
  - Subscribe button
  - Search with autocomplete
  - Watch history
- **Tech:** Next.js (App Router), SSR
- **Third-Party:** `react-player`, `react-icons`

---

### Task 41 — Admin Dashboard with RBAC UI 🔴

- **Goal:** Role-based admin panel
- **Requirements:**
  - Login with role selection (Admin, Editor, Viewer)
  - Sidebar navigation changes based on role
  - User management table (Admin only)
  - Content management (Admin + Editor)
  - Analytics view (All roles)
  - Role-based component rendering
  - Charts & statistics
  - Data export (CSV)
- **Tech:** Next.js, Middleware
- **Third-Party:** `recharts`, `@tanstack/react-table`, `zustand`

---

### Task 42 — Multi-Language (i18n) Website 🟡

- **Goal:** Website with Bengali, English, Arabic support
- **Requirements:**
  - Language switcher
  - RTL support for Arabic
  - Translated content for all pages
  - Date/number formatting per locale
  - URL-based locale (`/en/about`, `/bn/about`)
  - Persist language preference
- **Tech:** Next.js (App Router), i18n
- **Third-Party:** `next-intl`

---

### Task 43 — Form Builder (Drag & Drop) 🔴

- **Goal:** Google Forms style form builder
- **Requirements:**
  - Drag & drop form fields (text, textarea, select, checkbox, radio)
  - Field configuration (label, placeholder, required, options)
  - Reorder fields via drag & drop
  - Form preview mode
  - Form submission view
  - JSON export of form schema
  - Import JSON to recreate form
- **Tech:** React
- **Third-Party:** `@dnd-kit/core`, `@dnd-kit/sortable`, `react-hook-form`

---

### Task 44 — Code Snippet Manager 🟡

- **Goal:** GitHub Gist style code snippet saver
- **Requirements:**
  - Create snippet with title, language, code
  - Syntax highlighted code display
  - Copy to clipboard button
  - Filter by language
  - Search snippets
  - Tag system
  - LocalStorage persistence
- **Tech:** Next.js
- **Third-Party:** `prismjs` অথবা `react-syntax-highlighter`, `react-hot-toast`

---

### Task 45 — Weather Dashboard 🟡

- **Goal:** Beautiful weather app with data visualization
- **Requirements:**
  - Current weather display with icons
  - 7-day forecast
  - Hourly temperature chart
  - Search city
  - Geolocation support
  - Unit toggle (Celsius/Fahrenheit)
  - Weather-based dynamic background
  - Wind, Humidity, UV index cards
- **Tech:** Next.js (App Router)
- **Third-Party:** `recharts`, `react-icons`
- **API:** OpenWeatherMap API

---

### Task 46 — Recipe Finder App 🟡

- **Goal:** Recipe search & save application
- **Requirements:**
  - Search recipes by ingredient
  - Recipe detail page (ingredients, steps, nutrition)
  - Save favorite recipes (localStorage)
  - Filter by cuisine, diet type, cooking time
  - Meal planner (drag recipes to days)
  - Shopping list generator from selected recipes
- **Tech:** Next.js (App Router)
- **Third-Party:** `@dnd-kit/core`, `framer-motion`
- **API:** Spoonacular API অথবা TheMealDB

---

### Task 47 — Markdown Note-Taking App 🟡

- **Goal:** Notion-lite markdown note taking app
- **Requirements:**
  - Split view: editor + live preview
  - Markdown syntax support (headings, lists, code, links, images)
  - File tree sidebar (folders & notes)
  - Create, rename, delete notes
  - Full-text search across notes
  - LocalStorage persistence
  - Export as .md file
  - Keyboard shortcuts
- **Tech:** React
- **Third-Party:** `react-markdown`, `remark-gfm`, `react-syntax-highlighter`

---

### Task 48 — Fitness Tracker Dashboard 🔴

- **Goal:** Health & fitness tracking UI
- **Requirements:**
  - Daily workout log form
  - Weekly/Monthly stats with charts
  - Calorie tracker with progress ring
  - Exercise library with search
  - Workout plan builder
  - Progress photos timeline
  - Goal setting & achievement badges
  - Streak counter
- **Tech:** Next.js (App Router)
- **Third-Party:** `recharts`, `framer-motion`, `date-fns`, `zustand`

---

### Task 49 — Quiz/Assessment Platform 🟡

- **Goal:** Online quiz system
- **Requirements:**
  - Quiz creation form (MCQ, True/False, Short Answer)
  - Timer per question
  - Progress indicator
  - Instant feedback (correct/wrong)
  - Score calculation & result page
  - Review answers after quiz
  - Leaderboard UI
  - Animation on correct/wrong answer
- **Tech:** Next.js
- **Third-Party:** `framer-motion`, `react-confetti` (on perfect score), `zustand`

---

### Task 50 — Personal Finance Manager 🔴

- **Goal:** Complete personal finance tracking UI
- **Requirements:**
  - Income & expense entry form
  - Transaction list with filters (date, category, type)
  - Monthly budget setting per category
  - Dashboard with:
    - Income vs Expense bar chart
    - Category-wise pie chart
    - Monthly trend line chart
    - Budget vs Actual comparison
  - Recurring transaction support
  - Export transactions as CSV
  - Dark/Light mode
  - Responsive design
- **Tech:** Next.js (App Router)
- **Third-Party:** `recharts`, `zustand`, `react-hook-form`, `zod`, `date-fns`, `@tanstack/react-table`

---

## 📊 Task Summary

| Category              | Tasks | Count  |
| --------------------- | ----- | ------ |
| UI & CSS              | 1-5   | 5      |
| React Functionality   | 6-15  | 10     |
| React + Third-Party   | 16-25 | 10     |
| Next.js Core          | 26-35 | 10     |
| Advanced Combinations | 36-50 | 15     |
| **Total**             |       | **50** |

## 📦 Most Used Third-Party Libraries

| Library                   | Used In Tasks             |
| ------------------------- | ------------------------- |
| `react-hook-form` + `zod` | 8, 11, 33, 36, 38, 43, 50 |
| `framer-motion`           | 3, 24, 37, 46, 48, 49     |
| `zustand`                 | 36, 37, 41, 48, 49, 50    |
| `recharts`                | 16, 38, 41, 45, 48, 50    |
| `@dnd-kit/core`           | 6, 14, 38, 43, 46         |
| `react-icons`             | 2, 25, 40, 45             |
| `date-fns`                | 37, 38, 48, 50            |
| `@tanstack/react-table`   | 13, 41, 50                |
| `react-router-dom`        | 11, 24                    |
| `next-auth`               | 28                        |
| `next-intl`               | 42                        |
| `swiper`                  | 3, 36                     |

---

> 💡 **Tips:**
>
> - Easy (🟢) দিয়ে শুরু করো, তারপর Medium (🟡), শেষে Hard (🔴)
> - প্রতিটা task এ Git ব্যবহার করো — commit history maintain করো
> - প্রতিটা task আলাদা folder এ রাখো
> - Responsive design প্রতিটা task এ mandatory
> - TypeScript ব্যবহার করার চেষ্টা করো সব task এ
> - প্রতিটা task শেষে README.md লেখো — কী শিখলে, কী challenge ছিল
