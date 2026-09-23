# Facturas y Presupuestos

> Documentation repository for **Facturas y Presupuestos**, a desktop application to manage invoices and budgets, with multi-format document export.

This repository contains **project documentation only** (architecture, database schema, and changelog). The source code lives in a private repository.

## Overview

**Facturas y Presupuestos** is a desktop application built with Electron that lets you manage your customers, invoices (`facturas`) and budgets (`presupuestos`) in a single place. It stores everything locally in a SQLite database and exports documents to **PDF**, **Excel (.xlsx)** and **Word (.docx)** using configurable templates.

### Features

- **Customers** — create, edit, delete and search customers (name, address, tax ID).
- **Invoices** — full invoice lifecycle with line items, units, payment method, tax rate (VAT) and delivery-note references.
- **Budgets** — budgets with the same line-item model as invoices, promotable to invoices.
- **References** — delivery-note references linked to customers and documents.
- **Document export** — export invoices and budgets to PDF, Excel and Word using data-driven templates.
- **Settings** — company data, numbering for the next invoice/budget, export path (persisted) and PDF offset controls.
- **Dashboard (Home)** — recent customers/invoices/budgets, quick actions and counters.
- **Search & Selector** — customer search and a reusable keyboard-friendly selector component.

## Tech stack

| Layer | Technology |
| --- | --- |
| Desktop shell | Electron + Electron Forge |
| UI | React 19 + TypeScript |
| Build | Vite 5 |
| Styling | Tailwind CSS 4 |
| State / routing | React Store (Zustand-style stores) + React Router |
| Database | SQLite (local) |
| Exports | pdfkit (PDF), exceljs (XLSX), docxtemplater + pizzip (DOCX) |
| Animations | framer-motion |
| Linting | ESLint + `@typescript-eslint` |

## Project structure (source, private)

```
presupuestos_y_facturas_js/
├── programa/               # Main Electron app (Vite + Electron Forge)
│   ├── src/
│   │   ├── server/         # Main process: db, export, ipc, validator, setup
│   │   ├── types/          # Shared TypeScript types
│   │   └── src/            # Renderer process
│   │       ├── pages/      # Home, Customers, Invoices, Budgets, Settings
│   │       ├── components/ # UI components, modals, invoice form
│   │       ├── store/      # Client-side stores
│   │       ├── hooks/      # Custom React hooks
│   │       └── settings/   # Settings UI components
│   └── resources/          # App icons and installer assets
├── DB/                     # SQLite schema, templates, UML diagram
├── facturas/               # Legacy Electron app (webpack)
└── pruebas/                # Export tests / sample templates
```

## Documentation

| Document | Description |
| --- | --- |
| [Architecture](docs/architecture.md) | Electron processes, IPC flow and module breakdown |
| [Database schema](docs/database-schema.md) | Entities, relations, generated columns and triggers |
| [Changelog](docs/changelog.md) | Feature milestones derived from the development history |
| [Screenshots](docs/screenshots/) | *(placeholder — coming soon)* |

## Screenshots

> **TODO:** Screenshots of the application will be added here.

<!-- Screenshots will be placed in `docs/screenshots/` -->

## Requirements

- Node.js 18+
- npm (or yarn/pnpm)

> The application itself is distributed as an installer (Windows) built with Electron Forge.

## License

MIT
