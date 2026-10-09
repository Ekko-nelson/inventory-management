<template>
  <aside class="sidebar" :class="{ collapsed: isCollapsed }">
    <!-- Logo Area with Toggle -->
    <div class="sidebar-logo">
      <div v-if="!isCollapsed" class="logo-content">
        <h1 class="logo-text">{{ companyName }}</h1>
        <span v-if="subtitle" class="logo-subtitle">{{ subtitle }}</span>
      </div>
      <button
        class="collapse-toggle"
        @click="toggleCollapse"
        :aria-label="isCollapsed ? 'Expand sidebar' : 'Collapse sidebar'"
      >
        <span class="toggle-icon">{{ isCollapsed ? '→' : '←' }}</span>
      </button>
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
        :title="isCollapsed ? route.label : ''"
      >
        <span class="nav-item-icon">{{ route.icon }}</span>
        <span v-if="!isCollapsed" class="nav-item-label">{{ route.label }}</span>
      </router-link>
    </nav>

    <!-- Utilities Section -->
    <div class="sidebar-utilities">
      <div v-if="!isCollapsed" class="utilities-divider"></div>
      <slot name="utilities">
        <!-- LanguageSwitcher, ProfileMenu go here -->
      </slot>
    </div>
  </aside>
</template>

<script>
import { useRoute } from 'vue-router'
import { ref, watch } from 'vue'

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
      required: true
    }
  },
  setup() {
    const route = useRoute()
    const isCollapsed = ref(false)

    const isActiveRoute = (path) => {
      if (path === '/') {
        return route.path === '/'
      }
      return route.path.startsWith(path)
    }

    const toggleCollapse = () => {
      isCollapsed.value = !isCollapsed.value
    }

    // Update CSS variable when collapsed state changes
    watch(isCollapsed, (collapsed) => {
      document.documentElement.style.setProperty(
        '--current-sidebar-width',
        collapsed ? 'var(--sidebar-width-collapsed)' : 'var(--sidebar-width)'
      )
    }, { immediate: true })

    return {
      isActiveRoute,
      isCollapsed,
      toggleCollapse
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
  transition: width var(--transition-base);
}

.sidebar.collapsed {
  width: var(--sidebar-width-collapsed);
}

/* Logo Area */
.sidebar-logo {
  height: var(--header-height);
  padding: var(--space-4);
  border-bottom: 1px solid var(--sidebar-border);
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: var(--space-2);
}

.logo-content {
  flex: 1;
  min-width: 0;
  display: flex;
  flex-direction: column;
}

.logo-text {
  font-size: var(--text-lg);
  font-weight: var(--font-bold);
  color: #ffffff;
  margin: 0;
  letter-spacing: -0.025em;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.logo-subtitle {
  font-size: var(--text-xs);
  color: var(--sidebar-nav-item-text);
  margin-top: var(--space-1);
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.collapse-toggle {
  width: 32px;
  height: 32px;
  border: none;
  background: var(--sidebar-nav-item-bg-hover);
  color: var(--sidebar-nav-item-text);
  border-radius: var(--radius-md);
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all var(--transition-fast);
  flex-shrink: 0;
}

.collapse-toggle:hover {
  background: var(--color-slate-700);
  color: #ffffff;
}

.collapse-toggle:focus-visible {
  outline: 2px solid var(--brand-primary);
  outline-offset: 2px;
}

.toggle-icon {
  font-size: 1rem;
  line-height: 1;
}

.sidebar.collapsed .sidebar-logo {
  justify-content: center;
  padding: var(--space-4) var(--space-2);
}

/* Navigation */
.sidebar-nav {
  flex: 1;
  overflow-y: auto;
  overflow-x: hidden;
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
  white-space: nowrap;
}

.sidebar.collapsed .sidebar-nav-item {
  justify-content: center;
  padding: var(--space-3) var(--space-2);
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
  font-size: 1.25rem;
}

.nav-item-label {
  flex: 1;
  overflow: hidden;
  text-overflow: ellipsis;
}

/* Utilities */
.sidebar-utilities {
  padding: var(--space-2);
  border-top: 1px solid var(--sidebar-border);
}

.sidebar.collapsed .sidebar-utilities {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.utilities-divider {
  height: 1px;
  background: var(--sidebar-border);
  margin-bottom: var(--space-2);
}

/* Scrollbar styling */
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
