<template>
  <aside
    class="app-sidebar"
    :class="{ 'is-collapsed': collapsed, 'is-open': open }"
    aria-label="Primary"
  >
    <div class="brand">
      <span class="brand-name">{{ t('nav.companyName') }}</span>
      <span class="brand-subtitle">{{ t('nav.subtitle') }}</span>
    </div>
    <nav class="nav-list">
      <router-link to="/" class="nav-link nav-root" :title="t('nav.overview')" @click="$emit('navigate')">
        <svg class="nav-icon" viewBox="0 0 24 24" aria-hidden="true">
          <rect x="3" y="3" width="7" height="9" rx="1" /><rect x="14" y="3" width="7" height="5" rx="1" />
          <rect x="14" y="12" width="7" height="9" rx="1" /><rect x="3" y="16" width="7" height="5" rx="1" />
        </svg>
        <span class="nav-label">{{ t('nav.overview') }}</span>
      </router-link>
      <router-link to="/inventory" class="nav-link" :title="t('nav.inventory')" @click="$emit('navigate')">
        <svg class="nav-icon" viewBox="0 0 24 24" aria-hidden="true">
          <path d="M21 8l-9-5-9 5v8l9 5 9-5V8z" /><path d="M3 8l9 5 9-5" /><path d="M12 13v8" />
        </svg>
        <span class="nav-label">{{ t('nav.inventory') }}</span>
      </router-link>
      <router-link to="/orders" class="nav-link" :title="t('nav.orders')" @click="$emit('navigate')">
        <svg class="nav-icon" viewBox="0 0 24 24" aria-hidden="true">
          <path d="M9 4h6a1 1 0 011 1v1H8V5a1 1 0 011-1z" /><path d="M8 6H6a1 1 0 00-1 1v13a1 1 0 001 1h12a1 1 0 001-1V7a1 1 0 00-1-1h-2" />
          <path d="M9 12h6" /><path d="M9 16h4" />
        </svg>
        <span class="nav-label">{{ t('nav.orders') }}</span>
      </router-link>
      <router-link to="/spending" class="nav-link" :title="t('nav.finance')" @click="$emit('navigate')">
        <svg class="nav-icon" viewBox="0 0 24 24" aria-hidden="true">
          <circle cx="12" cy="12" r="9" /><path d="M14.5 9.5c-.5-1-1.4-1.5-2.5-1.5-1.4 0-2.5.8-2.5 2s1.1 1.7 2.5 2 2.5.8 2.5 2-1.1 2-2.5 2c-1.1 0-2-.5-2.5-1.5" /><path d="M12 6.5V8m0 8v1.5" />
        </svg>
        <span class="nav-label">{{ t('nav.finance') }}</span>
      </router-link>
      <router-link to="/demand" class="nav-link" :title="t('nav.demandForecast')" @click="$emit('navigate')">
        <svg class="nav-icon" viewBox="0 0 24 24" aria-hidden="true">
          <path d="M3 17l6-6 4 4 8-8" /><path d="M15 7h6v6" />
        </svg>
        <span class="nav-label">{{ t('nav.demandForecast') }}</span>
      </router-link>
      <router-link to="/reports" class="nav-link" title="Reports" @click="$emit('navigate')">
        <svg class="nav-icon" viewBox="0 0 24 24" aria-hidden="true">
          <path d="M14 3H7a2 2 0 00-2 2v14a2 2 0 002 2h10a2 2 0 002-2V8l-5-5z" /><path d="M14 3v5h5" /><path d="M9 13h6" /><path d="M9 17h6" />
        </svg>
        <span class="nav-label">Reports</span>
      </router-link>
    </nav>
  </aside>
</template>

<script>
import { useI18n } from '../composables/useI18n'

export default {
  name: 'AppSidebar',
  props: {
    collapsed: { type: Boolean, default: false },
    open: { type: Boolean, default: false }
  },
  emits: ['navigate'],
  setup() {
    const { t } = useI18n()
    return { t }
  }
}
</script>

<style scoped>
.app-sidebar {
  background: var(--sidebar-bg);
  border-right: 1px solid var(--color-border);
  width: var(--sidebar-width);
  height: 100vh;
  position: sticky;
  top: 0;
  display: flex;
  flex-direction: column;
  overflow-y: auto;
  overflow-x: hidden;
}

.brand {
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 2px;
  min-height: var(--topbar-height);
  padding: 0 var(--space-5);
  border-bottom: 1px solid var(--color-border);
}

.brand-name {
  font-size: var(--text-lg);
  font-weight: 700;
  color: var(--color-text);
  letter-spacing: -0.02em;
  white-space: nowrap;
}

.brand-subtitle {
  font-size: var(--text-xs);
  color: var(--color-text-muted);
  white-space: nowrap;
}

.nav-list {
  display: flex;
  flex-direction: column;
  gap: var(--space-1);
  padding: var(--space-4) var(--space-3);
}

.nav-link {
  display: flex;
  align-items: center;
  gap: var(--space-3);
  height: 40px;
  padding: 0 var(--space-3);
  border-radius: var(--radius-sm);
  color: var(--sidebar-text);
  font-size: var(--text-base);
  font-weight: 500;
  text-decoration: none;
  white-space: nowrap;
  transition: background-color 0.15s ease, color 0.15s ease;
}

.nav-link:hover {
  background: #f1f5f9;
  color: var(--color-text);
}

.nav-link:focus-visible {
  outline: 2px solid var(--color-accent);
  outline-offset: 2px;
}

/* "/" matches every route as a prefix, so it only counts as active on an exact match */
.nav-link:not(.nav-root).router-link-active,
.nav-link.nav-root.router-link-exact-active {
  background: var(--color-accent-soft);
  color: var(--color-accent);
}

.nav-icon {
  width: 18px;
  height: 18px;
  flex-shrink: 0;
  fill: none;
  stroke: currentColor;
  stroke-width: 1.8;
  stroke-linecap: round;
  stroke-linejoin: round;
}

/* Icon rail: labels visually hidden, title attribute still shows the name */
.is-collapsed { width: var(--sidebar-width-collapsed); }
.is-collapsed .brand { align-items: center; padding: 0 var(--space-2); }
.is-collapsed .brand-subtitle,
.is-collapsed .nav-label {
  position: absolute;
  width: 1px;
  height: 1px;
  overflow: hidden;
  clip: rect(0 0 0 0);
  white-space: nowrap;
}
.is-collapsed .brand-name { font-size: var(--text-sm); }
.is-collapsed .nav-link { justify-content: center; padding: 0; }

@media (min-width: 768px) and (max-width: 1023px) {
  .app-sidebar { width: var(--sidebar-width-collapsed); }
  .brand { align-items: center; padding: 0 var(--space-2); }
  .brand-subtitle,
  .nav-label {
    position: absolute;
    width: 1px;
    height: 1px;
    overflow: hidden;
    clip: rect(0 0 0 0);
    white-space: nowrap;
  }
  .brand-name { font-size: var(--text-sm); }
  .nav-link { justify-content: center; padding: 0; }
}

/* Phones: off-canvas overlay, above dropdowns (1000) and below modals (2000) */
@media (max-width: 767px) {
  .app-sidebar,
  .app-sidebar.is-collapsed {
    position: fixed;
    top: 0;
    left: 0;
    bottom: 0;
    width: var(--sidebar-width);
    z-index: 1100;
    transform: translateX(-100%);
    transition: transform 0.2s ease;
    box-shadow: none;
  }
  .app-sidebar.is-open {
    transform: translateX(0);
    box-shadow: var(--shadow-md);
  }
  .is-collapsed .brand { align-items: stretch; padding: 0 var(--space-5); }
  .is-collapsed .brand-subtitle,
  .is-collapsed .nav-label {
    position: static;
    width: auto;
    height: auto;
    overflow: visible;
    clip: auto;
  }
  .is-collapsed .brand-name { font-size: var(--text-lg); }
  .is-collapsed .nav-link { justify-content: flex-start; padding: 0 var(--space-3); }
}
</style>
