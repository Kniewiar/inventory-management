# Bug analysis: planted bugs (workshop exercise)

Found 2026-11-03 on branch `new_features`, before any fixes.
Method: code review plus checks against the running API (`localhost:8001`), and a compile
check of every page through Vite. Not yet confirmed by clicking through in a browser.

✅ = checked against the code by hand. Unmarked items are code-review findings that still need confirming in the UI.

## Visible in the app

| # | Page | Symptom | Cause | Location |
|---|---|---|---|---|
| 1 ✅ | Demand | "Increasing" card shows `++50.0%` | `getChangePercent` already adds `+`, and the template adds another | `client/src/views/Demand.vue:23`, `:175` |
| 2 ✅ | Dashboard → Order Health | Donut segments overlap (red wraps over green) | Segments scaled to 440, but circumference is 2π·65 ≈ 408 | `client/src/views/Dashboard.vue:595` (circles at `:88-101`) |
| 3 ✅ | Finance → Monthly Cost Flow | All monthly bars look identical/maxed | `getBarHeight` hard-codes max `25000`; real monthly costs are $135K–$750K; axis labels hard-coded `$25K…$0` | `client/src/views/Spending.vue:83-88`, `:386-389` |
| 4 ✅ | Finance → Spending by Category | Percentages sum to 123.6% and don't match amounts | Bad data: Raw Materials $2.56M = 42.5%, Components $2.95M = 38.8%, Equipment 19.0%, Consumables 23.3% | `server/data/spending.json` → `category_spending` |
| 5 ✅ | Inventory → row detail modal | "Warehouse" shows the shelf location (e.g. A-12) | Uses `inventoryItem.location` instead of `inventoryItem.warehouse` | `client/src/components/InventoryDetailModal.vue:84` |
| 6 ✅ | Dashboard → Inventory Shortages | "Create PO" does nothing | Checks `item.purchase_order_id`, but `/api/backlog` returns `has_purchase_order`; `<PurchaseOrderModal>` isn't imported and doesn't exist | `client/src/views/Dashboard.vue:213`, `:289`, `:669` |
| 7 ✅ | Dashboard → Tasks | Add/delete/toggle task does nothing | `api.js` calls `/api/tasks` and `/api/purchase-orders`, which don't exist on the backend (404) | `client/src/api.js:77-104` vs `server/main.py` |
| 8 | Dashboard → Top Products → detail | "undefined units" for Current Stock / Reorder Point | `topProducts` objects never set `quantityOnHand` / `reorderPoint` | `client/src/components/ProductDetailModal.vue:55,60`; `Dashboard.vue:510-519` |
| 9 | Any page in Japanese | `¥24.99`: dollar value with a yen sign | Only `utils/currency.js` converts (×150); other views just swap the symbol | `Inventory.vue:63-64`, `Orders.vue:59,71`, `Spending.vue:50-60,115,157`, `InventoryDetailModal`, `ProductDetailModal`, `CostDetailModal` |
| 10 | Backlog → item detail | "Expected Date" always N/A | `expected_date` isn't in the data or the `BacklogItem` model | `client/src/components/BacklogDetailModal.vue:66` |
| 11 | Dashboard → Top Products | List isn't really "top" | Sorted by earliest first-order date, then revenue, then cut to 12 | `client/src/views/Dashboard.vue:533-545` |

## Hidden / code quality

- **Index keys in `v-for`** (listed in the repo's CLAUDE.md "Common Issues"): `Orders.vue:57` uses `:key="idx"` (should be `item.sku`), and `Reports.vue:28,51,82` use `:key="index"` (should be `q.quarter` / `month.month`).
- **Revenue goal for quarters**: `Dashboard.vue:343-350` gives quarter filters (e.g. `Q1-2025`) the $800K single-month goal and an "MTD" label; it should be $2.4M. The FilterBar has no quarter options yet, so users can't hit this today.
- **Untranslated UI**: `Reports.vue` and `Backlog.vue` are hard-coded in English with `$`; `Demand.vue:22,41,60,90` don't call `translateProductName`; modal labels are English only. (The `en.js` and `ja.js` key sets match.)
- **Timezone off-by-one**: `Spending.vue:403-409` uses `new Date('2025-09-01')`, which parses as UTC, so US timezones show the previous day.
- **Quarter filter on Spending**: the month filter uses `split('-')[1]` and a `toISOString` comparison, which breaks for quarter values (revenue drops to $0, costs show the full year).
- **Fill-rate bar**: the Order Fill Rate progress bar width can go past 100% (`Dashboard.vue:43`).
- **Backlog SKUs aren't in `inventory.json`**: any warehouse/category filter empties the Shortages and Backlog lists.

## Checked and fine

- Backend Pydantic models match the JSON data.
- Warehouse, category and status filters are case-insensitive and work; month and quarter filters work on the API.
- Inventory correctly ignores the month filter (it has no time dimension).
- Every page compiles without errors.
