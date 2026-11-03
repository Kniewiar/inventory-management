# Starter design tokens

Use these when the project has no palette. If it has one (e.g. a CLAUDE.md design system), keep its colours and map them onto these names.

```css
:root {
  /* Colour: neutral slate, one blue accent */
  --color-bg: #f8fafc;          /* app background behind cards */
  --color-surface: #ffffff;     /* cards, sidebar, top bar */
  --color-border: #e2e8f0;
  --color-text: #0f172a;
  --color-text-muted: #64748b;
  --color-accent: #2563eb;
  --color-accent-soft: #eff6ff; /* active nav item background */
  --color-success: #059669;
  --color-warning: #d97706;
  --color-danger: #dc2626;
  --color-info: #0284c7;

  /* Sidebar (dark variant; for a light sidebar, use surface/text) */
  --sidebar-bg: #0f172a;
  --sidebar-text: #cbd5e1;
  --sidebar-text-active: #ffffff;
  --sidebar-item-active-bg: rgba(255, 255, 255, 0.08);

  /* Spacing: 4px scale */
  --space-1: 4px;
  --space-2: 8px;
  --space-3: 12px;
  --space-4: 16px;
  --space-5: 20px;
  --space-6: 24px;
  --space-8: 32px;

  /* Shape and depth */
  --radius-sm: 6px;
  --radius-md: 10px;
  --shadow-sm: 0 1px 2px rgba(15, 23, 42, 0.06);
  --shadow-md: 0 4px 12px rgba(15, 23, 42, 0.08);

  /* Type */
  --font-sans: -apple-system, BlinkMacSystemFont, "Segoe UI", Inter, sans-serif;
  --text-xs: 12px;
  --text-sm: 13px;
  --text-base: 14px;
  --text-lg: 16px;
  --text-xl: 20px;
  --text-2xl: 24px;   /* page title */

  /* Layout */
  --sidebar-width: 240px;
  --sidebar-width-collapsed: 72px;
  --topbar-height: 60px;
  --content-max-width: 1400px;
}
```

## Usage rules of thumb
- Card padding `--space-6`; grid gap `--space-6`; gap inside a card `--space-4`.
- Nav item: height 40px, padding `0 var(--space-3)`, gap between icon and label `--space-3`, radius `--radius-sm`.
- Page header: title `--text-2xl`/600, subtitle `--text-sm` muted, `--space-6` below.
- Table cell padding `var(--space-3) var(--space-4)`; header `--text-xs`, uppercase, letter-spacing 0.04em, muted.
