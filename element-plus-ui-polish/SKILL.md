---
name: "element-plus-ui-polish"
description: "Polishes Vue 3 + Element Plus UI to modern professional aesthetic. Invoke when user asks to improve UI look, make it less ugly, or more professional/modern."
---

# Element Plus UI Polish

This skill upgrades a default-themed Vue 3 + Element Plus application from "template-looking" to a modern, professional dashboard aesthetic. It applies principles gathered from dashboard design best practices and Element Plus theming guides.

## When to Invoke

- User says the UI looks "ugly", "basic", "template-like", or "土"
- User asks to make the UI more "professional", "modern", or "polished"
- User asks for UI/UX improvements on an Element Plus project

## Core Design Principles

1. **Restraint over flash**: Use muted, low-saturation backgrounds (#f5f7fa). Reserve bold colors exclusively for semantic meaning (primary actions, errors, success).
2. **Generous whitespace**: 8px grid system for consistent spacing. Components need room to breathe.
3. **Clear hierarchy**: Page title (20px, semibold) → Section headers (15px, semibold) → Body (14px) → Secondary text (12px, muted color).
4. **Soft elevation**: Cards with subtle shadows (`0 1px 3px rgba(0,0,0,0.06)`) instead of harsh borders. Hover states lift slightly (translateY -2px + shadow).
5. **White/light header**: Replace colored header bars with white background + subtle bottom border. Use brand color only for active navigation highlight.

## Implementation Steps

### Step 1: Create Global Style File (`src/style.css`)

Override Element Plus CSS variables and add design tokens:

```css
:root {
  --app-bg: #f5f7fa;
  --app-header-bg: #ffffff;
  --app-header-border: #e8ecf1;
  --app-header-text: #1d2129;
  --app-primary: #4c6ef5;
  --app-text: #1d2129;
  --app-text-secondary: #86909c;
  --app-card-shadow: 0 1px 3px rgba(0, 0, 0, 0.06), 0 1px 2px rgba(0, 0, 0, 0.04);
  --app-card-shadow-hover: 0 4px 12px rgba(0, 0, 0, 0.08);
  --app-radius: 10px;
}

body {
  margin: 0;
  background: var(--app-bg);
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
}

.el-card {
  border: none !important;
  border-radius: var(--app-radius) !important;
  box-shadow: var(--app-card-shadow) !important;
}

.el-table th.el-table__cell {
  background-color: #f8f9fb !important;
  font-weight: 600 !important;
  font-size: 13px !important;
}

.el-tag { border-radius: 6px !important; }
.el-button { border-radius: 6px !important; }
.el-input__wrapper { border-radius: 6px !important; }
.el-dialog { border-radius: 12px !important; }
```

Import in `main.js` after Element Plus CSS:
```js
import './style.css'
```

### Step 2: Redesign App.vue Header

Replace the default blue `el-header` with a clean white navigation bar:

- White background with subtle bottom shadow
- Logo text in dark color, brand color for icon only
- Navigation items: text-secondary color, primary color on active, 2px bottom border for active indicator
- Sticky header with z-index 100
- Icons for each nav item (from `@element-plus/icons-vue`)
- Max-width 1200px content area centered

### Step 3: Page-Level Consistency

Every page should follow this pattern:

```html
<div class="page-header">
  <h2 class="page-title">Page Title</h2>
  <div class="page-actions"><!-- buttons --></div>
</div>
<el-card><!-- content --></el-card>
```

Shared scoped styles for each page:
```css
.page-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 16px;
}
.page-title {
  font-size: 20px;
  font-weight: 600;
  color: var(--app-text);
  margin: 0;
}
```

### Step 4: Time Formatting

Never show raw ISO timestamps like `2026-05-20T10:47:18.565966`. Always format to human-readable:

```js
function formatTime(isoStr) {
  if (!isoStr) return '-'
  const d = new Date(isoStr)
  const pad = n => String(n).padStart(2, '0')
  return `${d.getFullYear()}-${pad(d.getMonth() + 1)}-${pad(d.getDate())} ${pad(d.getHours())}:${pad(d.getMinutes())}:${pad(d.getSeconds())}`
}
```

### Step 5: Task Duration Display

For task duration, compute on the backend and return as formatted string (e.g., "2m 30s", "1h 15m"). Display using `font-variant-numeric: tabular-nums` for alignment.

### Step 6: Cards as List Items (SchemeList pattern)

Instead of plain tables for list pages, use card grids:

```html
<el-row :gutter="16">
  <el-col :xs="24" :sm="12" :md="8" v-for="item in list">
    <el-card shadow="hover" @click="navigate" class="item-card">
      <h3>{{ item.name }}</h3>
      <p>{{ item.subtitle }}</p>
      <div class="card-actions" @click.stop>
        <!-- link-style buttons -->
      </div>
    </el-card>
  </el-col>
</el-row>
```

Card hover effect:
```css
.item-card {
  cursor: pointer;
  transition: transform 0.15s, box-shadow 0.15s;
}
.item-card:hover {
  transform: translateY(-2px);
  box-shadow: var(--app-card-shadow-hover) !important;
}
```

### Step 7: Action Buttons

Use `el-button link type="primary/danger/success"` for table row actions instead of solid buttons. This reduces visual noise.

### Step 8: Form Area Styling

Annotation class items in SchemeDetail should have subtle background + border:
```css
.class-item {
  border: 1px solid #edf0f5;
  border-radius: 8px;
  padding: 16px;
  margin-bottom: 12px;
  background: #fafbfc;
}
```

## Color Palette Reference

| Purpose | Color | Variable |
|---------|-------|----------|
| Background | #f5f7fa | --app-bg |
| Card/Header | #ffffff | --app-header-bg |
| Primary | #4c6ef5 | --app-primary |
| Text primary | #1d2129 | --app-text |
| Text secondary | #86909c | --app-text-secondary |
| Border subtle | #e8ecf1 | --app-header-border |
| Success | #40c057 | --app-success |
| Danger | #fa5252 | --app-danger |
| Warning | #fab005 | --app-warning |

## Anti-Patterns to Avoid

- ❌ Colored header bars (blue, green, etc.) — use white instead
- ❌ Raw ISO timestamp display — always format
- ❌ Solid buttons in table rows — use link-style
- ❌ Harsh borders on cards — use shadows instead
- ❌ Dense layouts with no spacing — 16px+ gaps between sections
- ❌ Using too many colors — stick to the palette above
- ❌ Small border-radius (2-4px) — use 6-10px for modern feel
