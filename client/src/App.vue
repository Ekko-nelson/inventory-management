<template>
  <div class="app">
    <Sidebar
      :company-name="t('nav.companyName')"
      :subtitle="t('nav.subtitle')"
      :routes="navigationRoutes"
    >
      <template #utilities>
        <LanguageSwitcher />
        <ProfileMenu
          @show-profile-details="showProfileDetails = true"
          @show-tasks="showTasks = true"
        />
      </template>
    </Sidebar>

    <div class="app-main">
      <header class="top-header"></header>
      <FilterBar />
      <main class="main-content">
        <router-view />
      </main>
    </div>

    <ProfileDetailsModal
      :is-open="showProfileDetails"
      @close="showProfileDetails = false"
    />

    <TasksModal
      :is-open="showTasks"
      :tasks="tasks"
      @close="showTasks = false"
      @add-task="addTask"
      @delete-task="deleteTask"
      @toggle-task="toggleTask"
    />
  </div>
</template>

<script>
import { ref, onMounted, computed } from 'vue'
import { api } from './api'
import { useAuth } from './composables/useAuth'
import { useI18n } from './composables/useI18n'
import Sidebar from './components/Sidebar.vue'
import FilterBar from './components/FilterBar.vue'
import ProfileMenu from './components/ProfileMenu.vue'
import ProfileDetailsModal from './components/ProfileDetailsModal.vue'
import TasksModal from './components/TasksModal.vue'
import LanguageSwitcher from './components/LanguageSwitcher.vue'

export default {
  name: 'App',
  components: {
    Sidebar,
    FilterBar,
    ProfileMenu,
    ProfileDetailsModal,
    TasksModal,
    LanguageSwitcher
  },
  setup() {
    const { currentUser } = useAuth()
    const { t } = useI18n()
    const showProfileDetails = ref(false)
    const showTasks = ref(false)
    const apiTasks = ref([])

    // Merge mock tasks from currentUser with API tasks
    const tasks = computed(() => {
      return [...currentUser.value.tasks, ...apiTasks.value]
    })

    const loadTasks = async () => {
      try {
        apiTasks.value = await api.getTasks()
      } catch (err) {
        console.error('Failed to load tasks:', err)
      }
    }

    const addTask = async (taskData) => {
      try {
        const newTask = await api.createTask(taskData)
        // Add new task to the beginning of the array
        apiTasks.value.unshift(newTask)
      } catch (err) {
        console.error('Failed to add task:', err)
      }
    }

    const deleteTask = async (taskId) => {
      try {
        // Check if it's a mock task (from currentUser)
        const isMockTask = currentUser.value.tasks.some(t => t.id === taskId)

        if (isMockTask) {
          // Remove from mock tasks
          const index = currentUser.value.tasks.findIndex(t => t.id === taskId)
          if (index !== -1) {
            currentUser.value.tasks.splice(index, 1)
          }
        } else {
          // Remove from API tasks
          await api.deleteTask(taskId)
          apiTasks.value = apiTasks.value.filter(t => t.id !== taskId)
        }
      } catch (err) {
        console.error('Failed to delete task:', err)
      }
    }

    const toggleTask = async (taskId) => {
      try {
        // Check if it's a mock task (from currentUser)
        const mockTask = currentUser.value.tasks.find(t => t.id === taskId)

        if (mockTask) {
          // Toggle mock task status
          mockTask.status = mockTask.status === 'pending' ? 'completed' : 'pending'
        } else {
          // Toggle API task
          const updatedTask = await api.toggleTask(taskId)
          const index = apiTasks.value.findIndex(t => t.id === taskId)
          if (index !== -1) {
            apiTasks.value[index] = updatedTask
          }
        }
      } catch (err) {
        console.error('Failed to toggle task:', err)
      }
    }

    const navigationRoutes = computed(() => [
      { path: '/', label: t('nav.overview'), icon: '📊' },
      { path: '/inventory', label: t('nav.inventory'), icon: '📦' },
      { path: '/orders', label: t('nav.orders'), icon: '🛒' },
      { path: '/spending', label: t('nav.finance'), icon: '💰' },
      { path: '/demand', label: t('nav.demandForecast'), icon: '📈' },
      { path: '/reports', label: 'Reports', icon: '📄' }
    ])

    onMounted(loadTasks)

    return {
      t,
      navigationRoutes,
      showProfileDetails,
      showTasks,
      tasks,
      addTask,
      deleteTask,
      toggleTask
    }
  }
}
</script>

<style>
:root {
  /* Color Primitives */
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
  --color-blue-50: #eff6ff;
  --color-blue-100: #dbeafe;
  --color-blue-500: #3b82f6;
  --color-blue-600: #2563eb;
  --color-blue-700: #1d4ed8;
  --color-green-500: #10b981;
  --color-green-600: #059669;
  --color-red-500: #ef4444;
  --color-red-600: #dc2626;
  --color-orange-500: #f59e0b;
  --color-orange-600: #ea580c;

  /* Spacing Scale */
  --space-1: 0.25rem;
  --space-2: 0.5rem;
  --space-3: 0.75rem;
  --space-4: 1rem;
  --space-5: 1.25rem;
  --space-6: 1.5rem;
  --space-8: 2rem;
  --space-10: 2.5rem;
  --space-12: 3rem;
  --space-16: 4rem;

  /* Typography */
  --text-xs: 0.75rem;
  --text-sm: 0.875rem;
  --text-base: 1rem;
  --text-lg: 1.125rem;
  --text-xl: 1.25rem;
  --text-2xl: 1.5rem;
  --text-3xl: 1.875rem;
  --font-normal: 400;
  --font-medium: 500;
  --font-semibold: 600;
  --font-bold: 700;
  --line-height-tight: 1.25;
  --line-height-normal: 1.5;
  --line-height-relaxed: 1.75;

  /* Border Radius */
  --radius-sm: 0.375rem;
  --radius-md: 0.5rem;
  --radius-lg: 0.625rem;
  --radius-xl: 0.75rem;

  /* Shadows */
  --shadow-sm: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
  --shadow-md: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
  --shadow-lg: 0 10px 15px -3px rgba(0, 0, 0, 0.1);

  /* Transitions */
  --transition-fast: 150ms ease;
  --transition-base: 200ms ease;
  --transition-slow: 300ms ease;

  /* Semantic Tokens */
  --surface-base: #ffffff;
  --surface-secondary: var(--color-slate-50);
  --bg-primary: var(--color-slate-50);
  --bg-secondary: #ffffff;
  --bg-tertiary: var(--color-slate-100);
  --text-primary: var(--color-slate-900);
  --text-secondary: var(--color-slate-600);
  --text-tertiary: var(--color-slate-500);
  --text-disabled: var(--color-slate-400);
  --border-light: var(--color-slate-200);
  --border-medium: var(--color-slate-300);
  --border-strong: var(--color-slate-400);
  --brand-primary: var(--color-blue-600);
  --brand-primary-hover: var(--color-blue-700);
  --brand-primary-subtle: var(--color-blue-50);
  --status-success: var(--color-green-600);
  --status-warning: var(--color-orange-600);
  --status-danger: var(--color-red-600);
  --status-info: var(--color-blue-600);

  /* Component Tokens */
  --sidebar-width: 16rem;
  --sidebar-width-collapsed: 4rem;
  --sidebar-bg: var(--color-slate-900);
  --sidebar-border: var(--color-slate-800);
  --sidebar-nav-item-text: var(--color-slate-400);
  --sidebar-nav-item-text-hover: #ffffff;
  --sidebar-nav-item-bg-hover: var(--color-slate-800);
  --sidebar-nav-item-active-text: #ffffff;
  --sidebar-nav-item-active-bg: var(--color-slate-800);
  --sidebar-nav-item-active-accent: var(--brand-primary);
  --header-height: 4rem;
  --header-bg: var(--surface-base);
  --header-border: var(--border-light);
  --content-max-width: 1600px;
  --content-padding-x: var(--space-8);
  --content-padding-y: var(--space-6);
  --card-bg: var(--surface-base);
  --card-border: var(--border-light);
  --card-border-hover: var(--border-medium);
  --card-shadow: var(--shadow-sm);
  --card-shadow-hover: var(--shadow-md);
  --z-base: 1;
  --z-dropdown: 1000;
  --z-sticky: 1020;
  --z-fixed: 1030;
  --z-modal-backdrop: 1040;
  --z-modal: 1050;
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
  background: var(--bg-primary);
  color: var(--text-primary);
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.app {
  display: flex;
  flex-direction: row;
  min-height: 100vh;
}

.app-main {
  flex: 1;
  margin-left: var(--current-sidebar-width, var(--sidebar-width));
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  transition: margin-left var(--transition-base);
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

.page-header {
  margin-bottom: var(--space-6);
}

.page-header h2 {
  font-size: var(--text-3xl);
  font-weight: var(--font-bold);
  color: var(--text-primary);
  margin-bottom: var(--space-2);
  letter-spacing: -0.025em;
}

.page-header p {
  color: var(--text-secondary);
  font-size: var(--text-sm);
}

.stats-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: var(--space-5);
  margin-bottom: var(--space-6);
}

.stat-card {
  background: var(--card-bg);
  padding: var(--space-5);
  border-radius: var(--radius-lg);
  border: 1px solid var(--card-border);
  transition: all var(--transition-base);
}

.stat-card:hover {
  border-color: var(--card-border-hover);
  box-shadow: var(--card-shadow-hover);
}

.stat-label {
  color: var(--text-secondary);
  font-size: var(--text-sm);
  font-weight: var(--font-semibold);
  text-transform: uppercase;
  letter-spacing: 0.5px;
  margin-bottom: var(--space-3);
}

.stat-value {
  font-size: 2.25rem;
  font-weight: var(--font-bold);
  color: var(--text-primary);
  letter-spacing: -0.025em;
}

.stat-card.warning .stat-value {
  color: var(--status-warning);
}

.stat-card.success .stat-value {
  color: var(--status-success);
}

.stat-card.danger .stat-value {
  color: var(--status-danger);
}

.stat-card.info .stat-value {
  color: var(--status-info);
}

.card {
  background: var(--card-bg);
  border-radius: var(--radius-lg);
  padding: var(--space-5);
  border: 1px solid var(--card-border);
  margin-bottom: var(--space-5);
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: var(--space-4);
  padding-bottom: var(--space-3);
  border-bottom: 1px solid var(--border-light);
}

.card-title {
  font-size: var(--text-lg);
  font-weight: var(--font-bold);
  color: var(--text-primary);
  letter-spacing: -0.025em;
}

.table-container {
  overflow-x: auto;
}

table {
  width: 100%;
  border-collapse: collapse;
}

thead {
  background: var(--bg-primary);
  border-top: 1px solid var(--border-light);
  border-bottom: 1px solid var(--border-light);
}

th {
  text-align: left;
  padding: var(--space-2) var(--space-3);
  font-weight: var(--font-semibold);
  color: var(--text-secondary);
  font-size: var(--text-xs);
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

td {
  padding: var(--space-2) var(--space-3);
  border-top: 1px solid var(--color-slate-100);
  color: var(--color-slate-700);
  font-size: var(--text-sm);
}

tbody tr {
  transition: background-color var(--transition-fast);
}

tbody tr:hover {
  background: var(--bg-primary);
}

.badge {
  display: inline-block;
  padding: var(--space-1) var(--space-3);
  border-radius: var(--radius-md);
  font-size: var(--text-xs);
  font-weight: var(--font-semibold);
  text-transform: uppercase;
  letter-spacing: 0.025em;
}

.badge.success {
  background: #d1fae5;
  color: #065f46;
}

.badge.warning {
  background: #fed7aa;
  color: #92400e;
}

.badge.danger {
  background: #fecaca;
  color: #991b1b;
}

.badge.info {
  background: var(--color-blue-100);
  color: #1e40af;
}

.badge.increasing {
  background: #d1fae5;
  color: #065f46;
}

.badge.decreasing {
  background: #fecaca;
  color: #991b1b;
}

.badge.stable {
  background: #e0e7ff;
  color: #3730a3;
}

.badge.high {
  background: #fecaca;
  color: #991b1b;
}

.badge.medium {
  background: #fed7aa;
  color: #92400e;
}

.badge.low {
  background: var(--color-blue-100);
  color: #1e40af;
}

.loading {
  text-align: center;
  padding: var(--space-12);
  color: var(--text-secondary);
  font-size: var(--text-sm);
}

.error {
  background: #fef2f2;
  border: 1px solid #fecaca;
  color: #991b1b;
  padding: var(--space-4);
  border-radius: var(--radius-md);
  margin: var(--space-4) 0;
  font-size: var(--text-sm);
}

@media (max-width: 768px) {
  .app-main {
    margin-left: 0;
  }

  .sidebar {
    transform: translateX(-100%);
    transition: transform var(--transition-base);
  }
}
</style>
