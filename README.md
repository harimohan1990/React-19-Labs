# React 19 Labs: View Transitions, Activity, and Experimental Features (April 2025)

React Labs posts give us a peek into experimental and upcoming features being developed by the React core team. As of April 23, 2025, React has released two powerful features for early testing: **View Transitions** and **Activity**. These are available in `react@experimental`.

---

## 🔄 View Transitions

**Purpose**: Animate UI transitions declaratively (e.g., navigation, reordering, or suspense boundaries).

### ✅ Installation

```bash
npm install react@experimental react-dom@experimental
```

### ✅ Usage

Wrap elements in `<ViewTransition>` to mark them for animation:

```tsx
<ViewTransition>
  <div>animate me</div>
</ViewTransition>
```

### ✅ Animation Triggers

React detects changes and triggers animation via:

* `startTransition(() => setState(...))`
* `useDeferredValue()`
* `<Suspense>`

### ✅ Animation Customization (CSS)

```css
::view-transition-old(*) {
  animation: 300ms ease-out fade-out;
}
::view-transition-new(*) {
  animation: 300ms ease-in fade-in;
}
```

### ✅ Shared Element Transitions

```tsx
<ViewTransition name={`video-${video.id}`}>
  <Thumbnail video={video} />
</ViewTransition>
```

Automatically animates matching elements across route/page changes.

### ✅ Cause-Based Transitions

```tsx
addTransitionType('nav-forward');
<ViewTransition share={{ 'nav-forward': 'slide-forward' }}>
  {heading}
</ViewTransition>
```

### ✅ Suspense + View Transitions

```tsx
<ViewTransition>
  <Suspense fallback={<Fallback />}>
    <Component />
  </Suspense>
</ViewTransition>
```

Animate the switch between fallback and final content.

### ✅ List Reordering

```tsx
{filteredList.map(item => (
  <ViewTransition key={item.id}>
    <ListItem item={item} />
  </ViewTransition>
))}
```

Enable with `useDeferredValue()`.

---

## 🔒 Activity Component

**Purpose**: Toggle visibility of UI while preserving internal state, useful for tabs, preloading, and off-screen content.

### ✅ Usage

```tsx
<Activity mode={isVisible ? 'visible' : 'hidden'}>
  <PageComponent />
</Activity>
```

### ✅ State Preservation Example

```tsx
<Activity mode={url === '/' ? 'visible' : 'hidden'}>
  <Home />
</Activity>
<Activity mode={url !== '/' ? 'visible' : 'hidden'}>
  <Details />
</Activity>
```

State like search filters or scroll position are preserved.

### ✅ Pre-rendering Pages

```tsx
{allPages.map(page => (
  <Activity mode={url === page.id ? 'visible' : 'hidden'}>
    <Page id={page.id} />
  </Activity>
))}
```

Reduces fallback delay for pre-fetched content.

### ✅ SSR Optimizations

* `mode="hidden"` excludes content from SSR
* `mode="visible"` prioritizes hydration

### Future Modes (In Exploration)

* `paused`: Visible in DOM but updates are paused
* `evict`: Automatically destroys state under memory pressure

---

## 🧪 Features in Active Development

### 📈 React Performance Tracks

* Adds custom tracks to devtools for performance profiling.

### ⚙️ Automatic Effect Dependencies (with Compiler)

* Automatically injects dependencies for `useEffect`
* Reduces manual mistakes and improves readability

### 💡 Compiler IDE Extension

* Provides feedback like inserted dependencies, diagnostics, and optimizations directly in the IDE.

### 🧩 Fragment Refs (Planned)

* Enables refs on `<Fragment>` groups.

### 👆 Gesture Animations

* WIP support for continuous, interactive animations tied to user gestures (e.g., swipe).

### 🌐 Concurrent Stores

* Reading external store data safely with `use(store)`
* Designed to support concurrent rendering without fallback glitches

---


