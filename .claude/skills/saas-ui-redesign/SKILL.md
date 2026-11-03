---
name: saas-ui-redesign
description: Redesign a Vue 3 app's UI into a modern SaaS-style interface. Replaces a top nav bar with a vertical left sidebar, introduces design tokens and a consistent spacing scale, and polishes cards, tables, forms and badges, without changing app behaviour. Use whenever the user wants to modernize, restyle, "make it look professional", add a sidebar, move the navigation to the left, clean up spacing, or give a Vue app a dashboard/SaaS look, even if they don't say "redesign".
---

# SaaS UI Redesign for Vue 3

Turn an existing Vue 3 app into a clean SaaS-style interface: a fixed left sidebar, a slim top bar, one spacing scale, and consistent surfaces. This is a **visual** change. Routes, data fetching, props/events, i18n keys and business logic stay exactly as they are, because users and tests depend on them and a redesign that breaks a feature is a regression, not an improvement.

## Workflow

### 1. Audit before touching anything
Find out how the UI is put together today. Read, don't edit:
- **App shell**: usually `App.vue`. Find the nav (`router-link`s), the logo, and header widgets (language switcher, profile menu, notifications), plus anything rendered between the header and `<router-view>` (e.g. a filter bar).
- **Routes**: `main.js` / `router/*.js`. List every route the nav must cover, and note any view file that has no route (don't add a nav item for it).
- **Where styles live**: global `<style>` in `App.vue`, `style.css`/`main.css`, or scoped per component. Note repeated colours, paddings and font sizes, since those become tokens.
- **Project rules**: read the repo's `CLAUDE.md` for a design system (colours, "no emojis", chart conventions) and for delegation rules (e.g. "delegate .vue changes to a vue-expert subagent"). Project rules win over this skill's defaults.

Summarise the findings to the user in a few lines and say what you'll change before you start.

### 2. Add design tokens
Define CSS custom properties once on `:root` in the global stylesheet, then use them everywhere. If the project already has a palette, map it onto these names instead of inventing new colours. Starting values are in `references/tokens.md`.

Cover at least: surface/background/border/text/muted colours, one accent, status colours (success/warning/danger/info), a **4px-based spacing scale** (`--space-1` = 4px … `--space-8` = 32px), radius, shadow, font sizes, and layout sizes (`--sidebar-width`, `--topbar-height`).

Why: consistent spacing is the single biggest "looks professional" lever, and it only stays consistent if every padding and gap comes from the same scale.

### 3. Build the layout shell
Replace the top header with this structure (adapt names to the codebase):

```
<div class="app-layout">            grid: [sidebar] [main column]
  <AppSidebar />                     fixed, full height, --sidebar-width
  <div class="app-main">
    <header class="app-topbar">      page title/breadcrumb left; existing widgets right
    <FilterBar />                    keep page-level bars that already existed
    <main class="app-content"><router-view /></main>
  </div>
</div>
```

**Sidebar (new `components/AppSidebar.vue`):**
- Brand/logo at the top, nav items in the middle, optional footer (user/profile or app version).
- Keep using `router-link`. Style the active state with the `router-link-active` / `router-link-exact-active` classes, or with the app's existing active check, so routing behaviour doesn't change. Use exact matching for `/` so "Overview" isn't always active.
- Each item: a simple inline SVG icon (stroke `currentColor`, 18–20px) plus the label. Inline SVG avoids a new dependency. Use emojis only if the project allows them.
- Keep every label going through the existing i18n function (`t('nav.x')`). If a label is hard-coded today, leave it hard-coded rather than adding keys the other locales lack.
- Clear hover, active (accent bar or tinted background) and keyboard focus (`:focus-visible`) states, so it's usable without a mouse.

**Top bar:** move the header widgets (language switcher, profile menu, etc.) here unchanged. Keep their props, emitted events and the modals they open. Optionally show the current page title from the route.

**Responsive:** below ~1024px, collapse the sidebar to icons only; below ~768px, hide it behind a menu button in the top bar, as an overlay with a backdrop that closes on route change. Content must never scroll horizontally at 375px width.

### 4. Apply the system to content
Work through the shared classes first (cards, stat/KPI cards, tables, badges, buttons, inputs, page headers), then each view:
- Replace hard-coded paddings, margins and gaps with the spacing tokens. Use one card padding and one grid gap throughout.
- Cards: surface background, 1px border, small radius, subtle shadow, consistent header row.
- Tables: muted uppercase header, comfortable row height, hover tint, right-aligned numbers.
- Typography: one page-title size, one section-title size, muted secondary text.
- Keep chart colours and status-colour meanings; only adjust spacing and containers around them.

Change CSS and markup structure only. If a template change is needed (e.g. wrapping something in a card), keep the same bindings, `v-if`/`v-for`, keys and event handlers.

### 5. Verify
- **Build**: run the project's build (e.g. `npm run build`) or make the dev server compile each changed file. No new console errors or Vue warnings.
- **Click through every route** from the sidebar. Check the active state, that data still loads, and that filters, modals, the language switch and the profile menu still work.
- **Check three widths**: ~1440, ~768 and ~375px.
- If browser tooling is available (e.g. Playwright MCP), take before/after screenshots. If not, tell the user exactly which pages to check.

### 6. Report back
Tell the user:
- the files added or changed, one line each;
- the tokens introduced and where they live;
- what to check by hand;
- anything you deliberately didn't change (e.g. existing bugs you noticed; mention them rather than fixing them silently).

## Guardrails
- Work on a feature branch; don't commit unless asked.
- Don't add UI libraries (Tailwind, Vuetify, icon packs) unless the user asks. The redesign should be reviewable CSS.
- Don't rename routes, components' public props/events, or i18n keys.
- Large views: change one view at a time and check it, rather than rewriting everything at once.
