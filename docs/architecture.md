# Architecture

**Facturas y Presupuestos** is an **Electron** desktop application. It follows Electron's standard multi-process model with a **main process** (Node.js) and a **renderer process** (React) communicating over **IPC**.

## Process model

```
┌────────────────────────────────────────────────────┐
│                    MAIN PROCESS                    │
│  (Node.js / Electron)                              │
│                                                    │
│  ┌─────────┐  ┌────────┐  ┌───────────┐            │
│  │  db.ts  │  │ export │  │ validator │            │
│  │ (SQLite)│  │  .ts   │  │    .ts    │            │
│  └────┬────┘  └───┬────┘  └─────┬─────┘            │
│       │           │             │                  │
│  ┌────┴───────────┴─────────────┴──────┐           │
│  │              ipc.ts                │           │
│  │   registers handlers (handle/on)   │           │
│  └────────────────┬───────────────────┘           │
└───────────────────┼───────────────────────────────┘
                    │ IPC (contextBridge / preload)
┌───────────────────┼───────────────────────────────┐
│                   ▼                               │
│                  RENDERER PROCESS                 │
│                (React 19 + Tailwind)              │
│                                                   │
│  pages/ → components/ → store/  ⇄  ipcRenderer    │
└───────────────────────────────────────────────────┘
```

## Modules

### Main process — `programa/src/server/`

| Module | Responsibility |
| --- | --- |
| `db.ts` | SQLite access layer: CRUD for customers, invoices, budgets, references, and line items. |
| `export.ts` | Document export to **PDF** (pdfkit), **Excel** (exceljs) and **Word** (docxtemplater + pizzip) using bundled templates. |
| `ipc.ts` | Registers all IPC handlers exposing the main-process API to the renderer. |
| `validator.ts` | Input validation and business rules before data is written to the database. |
| `setupScript.ts` | One-time setup tasks (DB initialization, template bundling). |

### Renderer process — `programa/src/src/`

| Layer | Description |
| --- | --- |
| `pages/` | Route-level screens: `HomePage`, `CustomersPage`, `InvoicesPage`, `BudgetPage`, `SettingsPage`. |
| `components/` | Reusable UI (`Selector`, `Modal`, `Toaster`, `PreviewItem`, `Separator`…), invoice form pieces and all creation/view modals for customers, invoices and budgets. |
| `store/` | Client-side state: `itemsStore` (documents/customers), `settingsStore`, `toastStore`. |
| `hooks/` | Custom hooks, e.g. `useFormFeedback`. |
| `settings/` | Settings UI controls (e.g. `Toggle`). |

## Data flow

1. The renderer calls a function exposed through the **preload bridge** (`ipcRenderer.invoke`).
2. `ipc.ts` receives the call, optionally validates input with `validator.ts`, and executes the operation.
3. `db.ts` runs SQL against the local SQLite database and returns the result.
4. The result travels back over IPC to the renderer, where the stores are updated and the UI re-renders.

Document export follows the same path: the renderer asks `ipc.ts`, which calls `export.ts` to produce a file in the configured (persisted) output directory.

## Persistence

- SQLite database stored locally. Invoices/budgets keep denormalised totals that are recalculated by **triggers** whenever line items change (see [database schema](database-schema.md)).
- Generated columns (`importe_factura`, `total_a_pagar`, `importe_linea`) compute amounts directly in the database.