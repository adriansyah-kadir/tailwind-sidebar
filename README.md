# tailwind-sidebar

A lightweight, CSS-only sidebar utility for **Tailwind CSS v4**.

Supports:

- Left, right, top, and bottom sidebars
- Static/collapsible sidebar
- Overlay/off-canvas sidebar
- Responsive sidebar behavior
- SSR-friendly default states
- CSS variable customization
- No JavaScript required

## Installation

Install the package:

```bash
npm install tailwind-sidebar
```

Then import it into your Tailwind CSS file:

```css
@import "tailwindcss";
@import "tailwind-sidebar";
```

## Basic Usage

The sidebar expects a container with an `aside` and a `main`:

```html
<div class="sidebar sidebar-left">
  <aside>
    Sidebar content
  </aside>

  <main>
    Main content
  </main>
</div>
```

By default, the sidebar is open.

To control its state, use the `data-open` attribute:

```html
<div class="sidebar sidebar-left" data-open="false">
  <aside>
    Sidebar content
  </aside>

  <main>
    Main content
  </main>
</div>
```

`data-open="false"` hides the sidebar, while `data-open="true"` shows it.

If `data-open` is omitted, the sidebar defaults to its normal open state.

## Directions

### Left

```html
<div class="sidebar sidebar-left">
  <aside>Sidebar</aside>
  <main>Content</main>
</div>
```

The sidebar is placed on the left.

### Right

```html
<div class="sidebar sidebar-right">
  <aside>Sidebar</aside>
  <main>Content</main>
</div>
```

The sidebar is placed on the right.

### Top

```html
<div class="sidebar sidebar-top">
  <aside>Header</aside>
  <main>Content</main>
</div>
```

The sidebar is placed above the main content.

### Bottom

```html
<div class="sidebar sidebar-bottom">
  <aside>Footer</aside>
  <main>Content</main>
</div>
```

The sidebar is placed below the main content.

## Static Sidebar

The default `sidebar` utility uses static positioning.

When closed, the sidebar collapses its width or height:

```html
<div class="sidebar sidebar-left" data-open="false">
  <aside>
    Sidebar
  </aside>

  <main>
    Content
  </main>
</div>
```

For a left or right sidebar, the default collapsed width is `0`.

For a top or bottom sidebar, the default collapsed height is `0`.

The main content remains part of the layout and expands into the space released by the sidebar.

## Overlay Sidebar

Use `sidebar-overlay` when the sidebar should float above the main content instead of occupying layout space.

```html
<div class="sidebar-overlay sidebar-left" data-open="false">
  <aside>
    Sidebar
  </aside>

  <main>
    Content
  </main>
</div>
```

When closed, the sidebar slides outside the container.

The default slide directions are:

```text
left    → translateX(-100%)
right   → translateX(100%)
top     → translateY(-100%)
bottom  → translateY(100%)
```

The overlay sidebar uses:

```css
position: absolute;
z-index: 10;
```

You can change these values with CSS variables.

## Responsive Sidebars

The `sidebar-hidden` and `sidebar-open` utilities are useful for defining different default states at different breakpoints.

For example, an overlay sidebar on mobile that becomes a normal sidebar on desktop:

```html
<div class="sidebar-overlay sidebar-left sidebar-hidden sm:sidebar-open">
  <aside>
    Sidebar
  </aside>

  <main>
    Content
  </main>
</div>
```

This provides an SSR-friendly default:

```text
Mobile
  → hidden

Desktop
  → open
```

The `data-open` attribute can still override the state.

### State behavior

`sidebar-hidden` applies when `data-open` is not explicitly `true`:

```html
<div class="sidebar sidebar-hidden">
```

`sidebar-open` applies when `data-open` is not explicitly `false`:

```html
<div class="sidebar sidebar-open">
```

This makes responsive defaults possible without requiring JavaScript to render the initial state.

For example:

```html
<div
  class="sidebar-overlay sidebar-left sidebar-hidden sm:sidebar-open"
  data-open={open}
>
```

The CSS provides a sensible state during SSR, while `data-open` can take control once the application state is available.

## State Utilities

### `sidebar-hidden`

Forces the sidebar into its hidden/collapsed state unless explicitly opened:

```html
<div class="sidebar sidebar-left sidebar-hidden">
```

Equivalent state:

```text
data-open != "true"
```

### `sidebar-open`

Forces the sidebar into its open state unless explicitly closed:

```html
<div class="sidebar sidebar-left sidebar-open">
```

Equivalent state:

```text
data-open != "false"
```

These utilities are especially useful with responsive variants:

```html
<div class="sidebar-hidden sm:sidebar-open">
```

## Customization

The sidebar is controlled using CSS custom properties.

### Size

The default sidebar size is `16rem`.

Override it with:

```html
<div
  class="sidebar sidebar-left"
  style="--sidebar-size: 20rem"
>
```

Or define it globally:

```css
@theme {
  --sidebar-size: 18rem;
}
```

### Separate Width and Height

You can override the computed dimensions independently:

```css
.sidebar {
  --sidebar-w: 20rem;
  --sidebar-h: 100%;
}
```

For a top sidebar:

```css
.sidebar {
  --sidebar-w: 100%;
  --sidebar-h: 12rem;
}
```

### Collapse Size

Static sidebars collapse using:

```css
--sidebar-collapse-to-w
--sidebar-collapse-to-h
```

For example:

```css
.sidebar {
  --sidebar-collapse-to-w: 3rem;
}
```

This allows a sidebar to collapse to a compact width instead of completely disappearing.

### Slide Distance

Overlay sidebars use:

```css
--sidebar-slide-to
```

For example:

```css
.sidebar {
  --sidebar-slide-to: translateX(-80%);
}
```

## Positioning

Overlay positioning can be customized with:

```css
--sidebar-pos
--sidebar-z
--sidebar-t
--sidebar-r
--sidebar-b
--sidebar-l
```

Example:

```css
.sidebar-overlay {
  --sidebar-z: 50;
  --sidebar-t: 1rem;
  --sidebar-b: 1rem;
}
```

## Ordering

The sidebar and main content are flex children.

You can change their order with:

```css
--sidebar-aside-order
--sidebar-main-order
```

For example:

```css
.sidebar {
  --sidebar-aside-order: 999;
  --sidebar-main-order: -999;
}
```

This can be useful for custom layouts beyond the built-in directional utilities.

## HTML Structure

The expected structure is:

```html
<div class="sidebar sidebar-left">
  <aside>
    <!-- Sidebar -->
  </aside>

  <main>
    <!-- Main content -->
  </main>
</div>
```

The utility styles the direct children:

```css
.sidebar > aside
.sidebar > main
```

Therefore, `aside` and `main` should be direct children of the sidebar container.

## Available Utilities

| Utility | Description |
|---|---|
| `sidebar` | Base sidebar layout |
| `sidebar-overlay` | Overlay/off-canvas sidebar |
| `sidebar-left` | Sidebar on the left |
| `sidebar-right` | Sidebar on the right |
| `sidebar-top` | Sidebar on the top |
| `sidebar-bottom` | Sidebar on the bottom |
| `sidebar-hidden` | Hidden/collapsed state |
| `sidebar-open` | Open state |

Utilities can be combined:

```html
<div class="sidebar sidebar-left">
```

or:

```html
<div class="sidebar-overlay sidebar-right sidebar-hidden">
```

## No JavaScript Required

`tailwind-sidebar` only provides CSS utilities.

It does not manage state, add event listeners, or provide a JavaScript API.

You can control the state using whatever framework or state-management solution you prefer:

```html
<div class="sidebar sidebar-left" data-open="false">
```

For Svelte:

```svelte
<div class="sidebar sidebar-left" data-open={open}>
```

For React:

```jsx
<div className="sidebar sidebar-left" data-open={open}>
```

## License

MIT
