---
name: saas-ui-redesign
description: Fully automated redesign of Vue 3 applications from horizontal top navigation to modern SaaS-style vertical sidebar navigation with comprehensive design system. Delegates to vue-expert subagent.
---

# SaaS UI Redesign Skill

Automatically transform Vue 3 applications from horizontal top navigation to modern SaaS-style vertical sidebar navigation with a comprehensive design system.

## Overview

This skill redesigns Vue 3 applications to use a modern SaaS interface pattern:
- **Vertical sidebar navigation** on the left (instead of horizontal tabs)
- **Comprehensive design system** using CSS variables (design tokens)
- **Dark sidebar** with professional styling (slate-900 background)
- **Improved layout** with better space utilization for content
- **Preserved functionality** - all routes, filters, components continue working

**When to use**: When you want to modernize a Vue 3 application's UI from horizontal navigation to a professional SaaS-style vertical sidebar layout.

**What it does**: 
1. Analyzes current navigation structure
2. Creates a new Sidebar component
3. Implements complete design token system
4. Transforms App.vue layout from column to row-based
5. Converts hardcoded styles to use CSS variables
6. Adds responsive mobile styles

**Delegation**: This skill delegates actual implementation to the `vue-expert` subagent with comprehensive instructions.

## How It Works

### Phase 1: Detection and Analysis

Read and analyze the current Vue 3 application structure:

```javascript
// Files to read
const filesToAnalyze = [
  '/client/src/App.vue',          // Current layout and navigation
  '/client/src/main.js',           // Router configuration
  '/client/src/components/FilterBar.vue',  // Positioning reference
  '/client/package.json'           // Vue version verification
]
```

**Navigation pattern detection**:
```javascript
// Search App.vue for these patterns
const horizontalNavPatterns = [
  'class="top-nav"',
  'class="nav-tabs"',
  '<nav',
  'flex-direction: row',
  'display: flex',
  '<router-link'
]

// Extract:
// - Route paths and labels
// - Utility components (ProfileMenu, LanguageSwitcher, etc.)
// - Current color palette
// - i18n key patterns
```

### Phase 2: Vue Expert Delegation

Spawn vue-expert subagent with comprehensive prompt containing:
- Complete design token system
- Full Sidebar.vue component code
- Exact transformation instructions
- Verification requirements

### Phase 3: Verification

After vue-expert completes:
- Verify all files created/modified
- Check for console errors
- Test routing functionality
- Validate design system implementation

## Design System Specification

### Complete CSS Variable Token System

Add this complete design token system to the top of `/client/src/App.vue` `<style>` section:

```css
:root {
  /* ============================================
     PRIMITIVE TOKENS - Foundation
     ============================================ */
  
  /* Color Primitives - Slate Scale */
  --color-slate-50: #f8fafc;
  --color-slate-100: #f1f5f9;
  --color-slate-200: #e2e8f0;
  --color-slate-300: #cbd5e1;
  --color-slate-400: #94a3b8;
  --color-slate-500: #64748b;
  --color-slate-600: #475569;
  --color-slate-700: #334155;
  --color-slate-800: #1e293b;
  --color-slate-900: #0f172a;
  
  /* Color Primitives - Blue (Primary) */
  --color-blue-50: #eff6ff;
  --color-blue-100: #dbeafe;
  --color-blue-500: #3b82f6;
  --color-blue-600: #2563eb;
  --color-blue-700: #1d4ed8;
  
  /* Color Primitives - Status Colors */
  --color-green-500: #10b981;
  --color-green-600: #059669;
  --color-red-500: #ef4444;
  --color-red-600: #dc2626;
  --color-orange-500: #f59e0b;
  --color-orange-600: #ea580c;
  
  /* Spacing Scale - 4px base unit */
  --space-1: 0.25rem;   /* 4px */
  --space-2: 0.5rem;    /* 8px */
  --space-3: 0.75rem;   /* 12px */
  --space-4: 1rem;      /* 16px */
  --space-5: 1.25rem;   /* 20px */
  --space-6: 1.5rem;    /* 24px */
  --space-8: 2rem;      /* 32px */
  --space-10: 2.5rem;   /* 40px */
  --space-12: 3rem;     /* 48px */
  --space-16: 4rem;     /* 64px */
  
  /* Typography Scale */
  --text-xs: 0.75rem;      /* 12px */
  --text-sm: 0.875rem;     /* 14px */
  --text-base: 1rem;       /* 16px */
  --text-lg: 1.125rem;     /* 18px */
  --text-xl: 1.25rem;      /* 20px */
  --text-2xl: 1.5rem;      /* 24px */
  --text-3xl: 1.875rem;    /* 30px */
  
  --font-normal: 400;
  --font-medium: 500;
  --font-semibold: 600;
  --font-bold: 700;
  
  --line-height-tight: 1.25;
  --line-height-normal: 1.5;
  --line-height-relaxed: 1.75;
  
  /* Border Radius */
  --radius-sm: 0.375rem;   /* 6px */
  --radius-md: 0.5rem;     /* 8px */
  --radius-lg: 0.625rem;   /* 10px */
  --radius-xl: 0.75rem;    /* 12px */
  
  /* Shadows */
  --shadow-sm: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
  --shadow-md: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
  --shadow-lg: 0 10px 15px -3px rgba(0, 0, 0, 0.1);
  
  /* Transitions */
  --transition-fast: 150ms ease;
  --transition-base: 200ms ease;
  --transition-slow: 300ms ease;
  
  /* ============================================
     SEMANTIC TOKENS - Purpose-based
     ============================================ */
  
  /* Surface Colors */
  --surface-base: #ffffff;
  --surface-raised: #ffffff;
  --surface-overlay: #ffffff;
  --surface-secondary: var(--color-slate-50);
  
  /* Background Colors */
  --bg-primary: var(--color-slate-50);
  --bg-secondary: #ffffff;
  --bg-tertiary: var(--color-slate-100);
  
  /* Text Colors */
  --text-primary: var(--color-slate-900);
  --text-secondary: var(--color-slate-600);
  --text-tertiary: var(--color-slate-500);
  --text-disabled: var(--color-slate-400);
  
  /* Border Colors */
  --border-light: var(--color-slate-200);
  --border-medium: var(--color-slate-300);
  --border-strong: var(--color-slate-400);
  
  /* Brand Colors */
  --brand-primary: var(--color-blue-600);
  --brand-primary-hover: var(--color-blue-700);
  --brand-primary-subtle: var(--color-blue-50);
  
  /* Status Colors */
  --status-success: var(--color-green-600);
  --status-success-subtle: #d1fae5;
  --status-warning: var(--color-orange-600);
  --status-warning-subtle: #fed7aa;
  --status-danger: var(--color-red-600);
  --status-danger-subtle: #fecaca;
  --status-info: var(--color-blue-600);
  --status-info-subtle: var(--color-blue-100);
  
  /* ============================================
     COMPONENT TOKENS - Specific use cases
     ============================================ */
  
  /* Sidebar */
  --sidebar-width: 16rem;         /* 256px */
  --sidebar-width-collapsed: 4rem; /* 64px - for future enhancement */
  --sidebar-bg: var(--color-slate-900);
  --sidebar-border: var(--color-slate-800);
  
  /* Sidebar Navigation */
  --sidebar-nav-item-text: var(--color-slate-400);
  --sidebar-nav-item-text-hover: #ffffff;
  --sidebar-nav-item-bg-hover: var(--color-slate-800);
  --sidebar-nav-item-active-text: #ffffff;
  --sidebar-nav-item-active-bg: var(--color-slate-800);
  --sidebar-nav-item-active-accent: var(--brand-primary);
  
  /* Header */
  --header-height: 4rem;          /* 64px */
  --header-bg: var(--surface-base);
  --header-border: var(--border-light);
  
  /* Main Content */
  --content-max-width: 1600px;
  --content-padding-x: var(--space-8);
  --content-padding-y: var(--space-6);
  
  /* Cards */
  --card-bg: var(--surface-base);
  --card-border: var(--border-light);
  --card-border-hover: var(--border-medium);
  --card-shadow: var(--shadow-sm);
  --card-shadow-hover: var(--shadow-md);
  
  /* Z-index Scale */
  --z-base: 1;
  --z-dropdown: 1000;
  --z-sticky: 1020;
  --z-fixed: 1030;
  --z-modal-backdrop: 1040;
  --z-modal: 1050;
  --z-popover: 1060;
  --z-tooltip: 1070;
}
```

### Style Conversion Reference

When converting existing hardcoded styles to use design tokens:

| Old Hardcoded Value | New CSS Variable |
|---------------------|------------------|
| `#ffffff` | `var(--surface-base)` |
| `#f8fafc` | `var(--bg-primary)` |
| `#f1f5f9` | `var(--color-slate-100)` |
| `#e2e8f0` | `var(--border-light)` |
| `#cbd5e1` | `var(--border-medium)` |
| `#94a3b8` | `var(--text-disabled)` |
| `#64748b` | `var(--text-secondary)` |
| `#475569` | `var(--color-slate-600)` |
| `#334155` | `var(--color-slate-700)` |
| `#1e293b` | `var(--color-slate-800)` |
| `#0f172a` | `var(--text-primary)` |
| `#2563eb` | `var(--brand-primary)` |
| `#3b82f6` | `var(--color-blue-500)` |
| `#10b981` | `var(--status-success)` |
| `#ef4444` | `var(--status-danger)` |
| `#f59e0b` | `var(--status-warning)` |
| `0.25rem` (4px) | `var(--space-1)` |
| `0.5rem` (8px) | `var(--space-2)` |
| `0.75rem` (12px) | `var(--space-3)` |
| `1rem` (16px) | `var(--space-4)` |
| `1.25rem` (20px) | `var(--space-5)` |
| `1.5rem` (24px) | `var(--space-6)` |
| `2rem` (32px) | `var(--space-8)` |
| `0.75rem` (12px font) | `var(--text-xs)` |
| `0.875rem` (14px font) | `var(--text-sm)` |
| `1rem` (16px font) | `var(--text-base)` |
| `1.125rem` (18px font) | `var(--text-lg)` |
| `1.25rem` (20px font) | `var(--text-xl)` |
| `1.5rem` (24px font) | `var(--text-2xl)` |
| `1.875rem` (30px font) | `var(--text-3xl)` |
| `6px` radius | `var(--radius-sm)` |
| `8px` radius | `var(--radius-md)` |
| `10px` radius | `var(--radius-lg)` |
| `12px` radius | `var(--radius-xl)` |

## Sidebar Component Template

### Complete Sidebar.vue Component

Create `/client/src/components/Sidebar.vue` with this complete implementation:

```vue
<template>
  <aside class="sidebar">
    <!-- Logo Area -->
    <div class="sidebar-logo">
      <h1 class="logo-text">{{ companyName }}</h1>
      <span v-if="subtitle" class="logo-subtitle">{{ subtitle }}</span>
    </div>

    <!-- Navigation -->
    <nav class="sidebar-nav" aria-label="Main navigation">
      <router-link
        v-for="route in routes"
        :key="route.path"
        :to="route.path"
        class="sidebar-nav-item"
        :class="{ active: isActiveRoute(route.path) }"
        :aria-current="isActiveRoute(route.path) ? 'page' : undefined"
      >
        <span class="nav-item-icon" v-if="route.icon">
          {{ route.icon }}
        </span>
        <span class="nav-item-label">{{ route.label }}</span>
      </router-link>
    </nav>

    <!-- Utilities Section -->
    <div class="sidebar-utilities">
      <div class="utilities-divider"></div>
      <slot name="utilities">
        <!-- LanguageSwitcher, ProfileMenu go here -->
      </slot>
    </div>
  </aside>
</template>

<script>
import { useRoute } from 'vue-router'

export default {
  name: 'Sidebar',
  props: {
    companyName: {
      type: String,
      required: true
    },
    subtitle: {
      type: String,
      default: ''
    },
    routes: {
      type: Array,
      required: true,
      // Expected format: [{ path: '/', label: 'Dashboard', icon?: '' }]
    }
  },
  setup() {
    const route = useRoute()
    
    const isActiveRoute = (path) => {
      // Exact match for root path
      if (path === '/') {
        return route.path === '/'
      }
      // Prefix match for all other paths
      return route.path.startsWith(path)
    }
    
    return {
      isActiveRoute
    }
  }
}
</script>

<style scoped>
.sidebar {
  position: fixed;
  top: 0;
  left: 0;
  width: var(--sidebar-width);
  height: 100vh;
  background: var(--sidebar-bg);
  border-right: 1px solid var(--sidebar-border);
  display: flex;
  flex-direction: column;
  z-index: var(--z-fixed);
}

/* Logo Area */
.sidebar-logo {
  height: var(--header-height);
  padding: var(--space-4);
  border-bottom: 1px solid var(--sidebar-border);
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.logo-text {
  font-size: var(--text-lg);
  font-weight: var(--font-bold);
  color: #ffffff;
  margin: 0;
  letter-spacing: -0.025em;
}

.logo-subtitle {
  font-size: var(--text-xs);
  color: var(--sidebar-nav-item-text);
  margin-top: var(--space-1);
}

/* Navigation */
.sidebar-nav {
  flex: 1;
  overflow-y: auto;
  padding: var(--space-4) var(--space-2);
}

.sidebar-nav-item {
  display: flex;
  align-items: center;
  gap: var(--space-3);
  padding: var(--space-3) var(--space-4);
  color: var(--sidebar-nav-item-text);
  text-decoration: none;
  font-size: var(--text-sm);
  font-weight: var(--font-medium);
  border-radius: var(--radius-md);
  transition: all var(--transition-fast);
  margin-bottom: var(--space-1);
  position: relative;
}

.sidebar-nav-item:hover {
  color: var(--sidebar-nav-item-text-hover);
  background: var(--sidebar-nav-item-bg-hover);
}

.sidebar-nav-item.active {
  color: var(--sidebar-nav-item-active-text);
  background: var(--sidebar-nav-item-active-bg);
}

.sidebar-nav-item.active::before {
  content: '';
  position: absolute;
  left: 0;
  top: 0;
  bottom: 0;
  width: 3px;
  background: var(--sidebar-nav-item-active-accent);
  border-radius: 0 var(--radius-sm) var(--radius-sm) 0;
}

.sidebar-nav-item:focus-visible {
  outline: 2px solid var(--brand-primary);
  outline-offset: 2px;
}

.nav-item-icon {
  width: 20px;
  height: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.nav-item-label {
  flex: 1;
}

/* Utilities */
.sidebar-utilities {
  padding: var(--space-2);
  border-top: 1px solid var(--sidebar-border);
}

.utilities-divider {
  height: 1px;
  background: var(--sidebar-border);
  margin-bottom: var(--space-2);
}

/* Scrollbar styling for sidebar nav */
.sidebar-nav::-webkit-scrollbar {
  width: 4px;
}

.sidebar-nav::-webkit-scrollbar-track {
  background: transparent;
}

.sidebar-nav::-webkit-scrollbar-thumb {
  background: var(--color-slate-700);
  border-radius: 2px;
}

.sidebar-nav::-webkit-scrollbar-thumb:hover {
  background: var(--color-slate-600);
}
</style>
```

## Layout Transformation Strategy

### App.vue Transformation

#### Before: Horizontal Navigation
```vue
<template>
  <div class="app">
    <header class="top-nav">
      <div class="nav-container">
        <div class="logo">
          <h1>{{ t('nav.companyName') }}</h1>
          <span>{{ t('nav.subtitle') }}</span>
        </div>
        <nav class="nav-tabs">
          <router-link to="/">{{ t('nav.overview') }}</router-link>
          <router-link to="/inventory">{{ t('nav.inventory') }}</router-link>
          <!-- ... more routes ... -->
        </nav>
        <LanguageSwitcher />
        <ProfileMenu />
      </div>
    </header>
    <FilterBar />
    <main class="main-content">
      <router-view />
    </main>
  </div>
</template>

<style>
.app {
  display: flex;
  flex-direction: column;  /* Vertical stacking */
  min-height: 100vh;
}
</style>
```

#### After: Vertical Sidebar
```vue
<template>
  <div class="app">
    <Sidebar 
      :company-name="t('nav.companyName')"
      :subtitle="t('nav.subtitle')"
      :routes="navigationRoutes"
    >
      <template #utilities>
        <LanguageSwitcher class="sidebar-utility" />
        <ProfileMenu
          class="sidebar-utility"
          @show-profile-details="showProfileDetails = true"
          @show-tasks="showTasks = true"
        />
      </template>
    </Sidebar>

    <div class="app-main">
      <header class="top-header">
        <!-- Optional: Search, notifications, etc. -->
      </header>
      <FilterBar />
      <main class="main-content">
        <router-view />
      </main>
    </div>

    <!-- Existing modals -->
    <ProfileDetailsModal ... />
    <TasksModal ... />
  </div>
</template>

<script>
export default {
  components: {
    Sidebar,
    // ... other components
  },
  setup() {
    const { t } = useI18n()
    
    // NEW: Navigation routes configuration
    const navigationRoutes = computed(() => [
      { path: '/', label: t('nav.overview') },
      { path: '/inventory', label: t('nav.inventory') },
      { path: '/orders', label: t('nav.orders') },
      { path: '/spending', label: t('nav.finance') },
      { path: '/demand', label: t('nav.demandForecast') },
      { path: '/reports', label: 'Reports' }
    ])
    
    return {
      t,
      navigationRoutes,
      // ... other returns
    }
  }
}
</script>

<style>
/* Design tokens added at top */
:root {
  /* ... complete token system ... */
}

.app {
  display: flex;
  flex-direction: row;  /* Horizontal: sidebar + main */
  min-height: 100vh;
}

.app-main {
  flex: 1;
  margin-left: var(--sidebar-width);
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

.top-header {
  background: var(--header-bg);
  border-bottom: 1px solid var(--header-border);
  height: var(--header-height);
  display: flex;
  align-items: center;
  padding: 0 var(--content-padding-x);
  position: sticky;
  top: 0;
  z-index: var(--z-sticky);
}

.main-content {
  flex: 1;
  max-width: var(--content-max-width);
  width: 100%;
  margin: 0 auto;
  padding: var(--content-padding-y) var(--content-padding-x);
}

/* Convert all hardcoded styles to use CSS variables */
/* ... */

/* Responsive: Mobile */
@media (max-width: 768px) {
  .app-main {
    margin-left: 0;
  }
  
  .sidebar {
    transform: translateX(-100%);
    transition: transform var(--transition-base);
  }
  
  .sidebar.open {
    transform: translateX(0);
  }
}
</style>
```

### FilterBar Adjustments

Minor updates needed in `/client/src/components/FilterBar.vue`:

```css
/* Update sticky positioning */
.filters-bar {
  position: sticky;
  top: var(--header-height);  /* Adjust to match new header height */
  z-index: var(--z-sticky);
  /* ... rest of styles ... */
}
```

## Vue Expert Instructions

When this skill is invoked, delegate to vue-expert with this comprehensive prompt:

```markdown
You are implementing a complete SaaS-style UI redesign for this Vue 3 application. Transform the current horizontal navigation to a modern vertical sidebar layout with a comprehensive design system.

## Context
- **Framework**: Vue 3 with Composition API
- **Router**: Vue Router
- **Current Navigation**: Horizontal top navigation in App.vue
- **Target**: Vertical sidebar navigation (SaaS-style)
- **Files to modify**: App.vue, FilterBar.vue
- **Files to create**: Sidebar.vue

## Implementation Steps (Execute in Order)

### 1. Add Design System to App.vue

Add the complete CSS variable token system to the top of `/client/src/App.vue` `<style>` section (before any other styles):

[Include complete CSS variable system from "Design System Specification" section above]

### 2. Create Sidebar Component

Create `/client/src/components/Sidebar.vue` with the complete implementation:

[Include complete Sidebar.vue component from "Sidebar Component Template" section above]

### 3. Transform App.vue Layout

**Template changes**:
- Import Sidebar component
- Change `.app` wrapper structure:
  - OLD: `<header class="top-nav">` with horizontal nav tabs
  - NEW: `<Sidebar>` component with utilities slot
- Add `.app-main` wrapper around header, FilterBar, and main content
- Create `navigationRoutes` computed property extracting routes from current router-links
- Move LanguageSwitcher and ProfileMenu to Sidebar utilities slot

**Script changes**:
```javascript
import Sidebar from './components/Sidebar.vue'

// In components object:
components: {
  Sidebar,
  // ... existing components
}

// In setup():
const navigationRoutes = computed(() => [
  { path: '/', label: t('nav.overview') },
  { path: '/inventory', label: t('nav.inventory') },
  { path: '/orders', label: t('nav.orders') },
  { path: '/spending', label: t('nav.finance') },
  { path: '/demand', label: t('nav.demandForecast') },
  { path: '/reports', label: 'Reports' }
])

// Add to return:
return {
  navigationRoutes,
  // ... existing returns
}
```

**Style changes**:
```css
/* Change .app layout */
.app {
  display: flex;
  flex-direction: row;  /* Changed from column */
  min-height: 100vh;
}

/* Add .app-main wrapper */
.app-main {
  flex: 1;
  margin-left: var(--sidebar-width);
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

/* Update header (rename from .top-nav to .top-header) */
.top-header {
  background: var(--header-bg);
  border-bottom: 1px solid var(--header-border);
  height: var(--header-height);
  display: flex;
  align-items: center;
  padding: 0 var(--content-padding-x);
  position: sticky;
  top: 0;
  z-index: var(--z-sticky);
}

/* Remove old .top-nav, .nav-container, .logo, .nav-tabs styles */
```

### 4. Update FilterBar Component

Modify `/client/src/components/FilterBar.vue` styles:

```css
.filters-bar {
  position: sticky;
  top: var(--header-height);  /* Update from 70px to use CSS variable */
  /* ... keep rest of styles ... */
}
```

### 5. Convert Hardcoded Styles to CSS Variables

Replace all hardcoded color, spacing, and typography values in App.vue global styles with CSS variables.

**Pattern examples**:
```css
/* OLD */
.card {
  background: #ffffff;
  padding: 1.25rem;
  border: 1px solid #e2e8f0;
  border-radius: 10px;
  box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
}

/* NEW */
.card {
  background: var(--card-bg);
  padding: var(--space-5);
  border: 1px solid var(--card-border);
  border-radius: var(--radius-lg);
  box-shadow: var(--card-shadow);
}
```

Use the conversion reference table provided earlier in this skill.

### 6. Add Responsive Styles

Add mobile breakpoint at end of App.vue styles:

```css
@media (max-width: 768px) {
  .app-main {
    margin-left: 0;
  }
  
  .sidebar {
    transform: translateX(-100%);
    transition: transform var(--transition-base);
  }
  
  /* Future: Add .sidebar.open state with toggle button */
}
```

## Verification Requirements

After implementation, verify:

✅ **Files Created**:
- `/client/src/components/Sidebar.vue` exists with complete implementation

✅ **Files Modified**:
- `/client/src/App.vue` - Layout transformed, design system added
- `/client/src/components/FilterBar.vue` - Positioning updated

✅ **Visual**:
- Sidebar visible on left with dark background (slate-900)
- Logo/company name at top of sidebar
- All navigation items listed vertically
- Active route has blue left border accent
- Hover states work on navigation items
- Main content shifted right by 256px
- Header simplified (no nav tabs)
- FilterBar still sticky below header

✅ **Functional**:
- Clicking sidebar nav items routes correctly
- Active route highlighting works
- Browser back/forward updates active state
- Language switcher works in sidebar
- Profile menu works in sidebar
- All filters continue working
- All modals continue working
- No console errors

✅ **Code Quality**:
- CSS variables used throughout (no hardcoded colors/spacing)
- Sidebar component uses scoped styles
- Active route detection uses Vue Router properly
- Props validated with types
- i18n keys work for all labels

## Summary

Provide a brief summary when complete:
- List files created
- List files modified
- Confirm all verification checks passed
- Note any warnings or issues encountered
```

## Verification Checklist

After vue-expert completes the transformation, verify:

### File Verification
- [ ] `.claude/skills/saas-ui-redesign/SKILL.md` created (this file)
- [ ] `/client/src/components/Sidebar.vue` created
- [ ] `/client/src/App.vue` modified (layout + design system)
- [ ] `/client/src/components/FilterBar.vue` modified (positioning)

### Visual Verification
- [ ] Sidebar displays on left side (256px wide, dark background)
- [ ] Logo text at top of sidebar
- [ ] All navigation items listed vertically
- [ ] Active route has blue accent border on left edge
- [ ] Hover states work (lighter background on hover)
- [ ] Main content area shifted right appropriately
- [ ] Header simplified (no horizontal navigation tabs)
- [ ] FilterBar still sticky below header
- [ ] Language switcher in sidebar bottom section
- [ ] Profile menu in sidebar bottom section

### Functional Verification
- [ ] Click each sidebar navigation item → routes to correct page
- [ ] Browser back button → active route state updates
- [ ] Browser forward button → active route state updates
- [ ] Refresh page → active route persists correctly
- [ ] Change language → sidebar labels update via i18n
- [ ] Open profile menu → dropdown displays correctly
- [ ] Apply filters → data filters correctly
- [ ] Open modals → all modals work (Profile, Tasks, etc.)
- [ ] Scroll main content → sidebar stays fixed
- [ ] Resize browser window → responsive behavior works

### Code Quality Verification
- [ ] No hardcoded colors in App.vue (all use CSS variables)
- [ ] No hardcoded spacing values (all use design tokens)
- [ ] Sidebar component uses scoped styles properly
- [ ] Active route detection uses `route.path` correctly
- [ ] Root path (`/`) uses exact match, others use prefix match
- [ ] All props have proper type validation
- [ ] No console errors in browser
- [ ] No console warnings in browser
- [ ] All composables work (useFilters, useAuth, useI18n)

### Design System Verification
- [ ] CSS variables defined in :root
- [ ] Primitive tokens defined (colors, spacing, typography)
- [ ] Semantic tokens defined (text, surface, border)
- [ ] Component tokens defined (sidebar, header, card)
- [ ] Token count > 50 variables (`grep -c "var(--" App.vue`)

## Edge Cases and Considerations

### Mobile Responsiveness
- Sidebar hidden by default on mobile (<768px)
- `.app-main` takes full width on mobile
- Future enhancement: Add hamburger menu toggle

### Internationalization (i18n)
- All sidebar labels use `t()` function with i18n keys
- navigationRoutes computed property reactively updates on language change
- Logo text and subtitle should be translatable

### Route Edge Cases
- Root path (`/`): Active only when exactly at root (exact match)
- Other paths: Active when current path starts with route path (prefix match)
- Nested routes: Handled by `startsWith()` logic
- Dynamic routes: Work with current detection approach

### Accessibility
- Semantic HTML: `<aside>`, `<nav>` elements
- ARIA labels: `aria-label="Main navigation"`
- ARIA current: `aria-current="page"` on active route
- Keyboard navigation: Tab and Enter keys work
- Focus indicators: Visible outline on focus

### Browser Compatibility
- CSS variables: Supported in all modern browsers
- Flexbox: Universal support
- Fixed positioning: Universal support
- IE11: Would need fallback values (if required)

## Testing the Skill

### Test on Current App

1. Ensure app is in initial state (horizontal navigation)
2. Run the skill: Invoke with `/saas-ui-redesign`
3. Wait for vue-expert to complete transformation
4. Open browser to `http://localhost:3000`
5. Run through verification checklist
6. Test all routes, filters, and interactions

### Expected Results

**Before**: Horizontal tabs in top nav, light background, logo on left
**After**: Dark vertical sidebar on left, main content shifted right, all functionality preserved

### Common Issues

**Issue**: Sidebar not visible
- Check import statement in App.vue
- Verify Sidebar.vue created in correct location
- Check console for component registration errors

**Issue**: Active route not highlighting
- Verify `isActiveRoute()` function in Sidebar.vue
- Check that route paths match exactly
- Ensure Vue Router is working

**Issue**: Styles not applying
- Verify CSS variables added to App.vue
- Check that `:root` block is at top of `<style>` section
- Clear browser cache and hard reload

**Issue**: FilterBar positioning wrong
- Check `top` value uses `var(--header-height)`
- Verify it's inside `.app-main` wrapper
- Check z-index values

## Future Enhancements

### Collapsible Sidebar
```javascript
// Add to Sidebar.vue
const collapsed = ref(false)
const toggleSidebar = () => {
  collapsed.value = !collapsed.value
}
```

```css
.sidebar.collapsed {
  width: var(--sidebar-width-collapsed); /* 64px */
}

.sidebar.collapsed .nav-item-label {
  display: none;
}
```

### Icon Support
- Integrate icon library (e.g., Heroicons, Lucide)
- Add icon prop to route objects
- Render icons in navigation items

### Nested Navigation
- Support route groups with children
- Collapsible submenus
- Indentation for hierarchy

### Dark Mode Toggle
- Add theme switcher component
- CSS variables for dark theme
- `prefers-color-scheme` media query

## Summary

This skill provides a complete, automated transformation of Vue 3 applications from horizontal to vertical sidebar navigation with a professional SaaS design. It:

✅ Implements comprehensive design system with CSS variables
✅ Creates reusable Sidebar component with proper Vue patterns
✅ Transforms App.vue layout while preserving all functionality
✅ Delegates implementation to vue-expert subagent
✅ Includes complete verification checklist
✅ Handles edge cases (mobile, i18n, accessibility)
✅ Provides clear before/after examples
✅ Documents troubleshooting steps

**Invoke with**: `/saas-ui-redesign`

**Expected duration**: 2-5 minutes depending on app complexity

**Result**: Modern SaaS-style interface with vertical sidebar navigation and comprehensive design system.
