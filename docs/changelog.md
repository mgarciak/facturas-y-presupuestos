# Changelog

Feature milestones reconstructed from the development history of the private source repository. Dates are not included; the entries are ordered chronologically (earliest → latest).

## Foundation

- **Initial commit** — project scaffold.
- **Python prototype for DOCX export** — first template export prototype (later replaced by the native Electron pipeline).
- **Electron app bootstrap** — first Electron application.
- **Database + XLSX template** — initial SQLite schema and first Excel template.
- **Home page, router and Zustand persistence** — routing between pages and persistent client-side state.
- **Webpack → Vite migration** — migrated the build pipeline from webpack to Vite.
- **Database & IPC** — database access layer exposed to the renderer over IPC.
- **Template-based first invoice generation**.

## Customers

- **Customers section** — first functional section.
- **Customer creation modal**, **toasts** and **modal infrastructure**.
- **Full CRUD for customers** — adding, editing and deleting.
- **Customer search** — searching and filtering customers, refined over several iterations.
- **Customer UI improvements** — fixed section text, better usability and keyboard-friendly selector integration.

## Invoices

- **Invoice creation and overview** — create invoices and browse a list of existing ones.
- **Invoice creation modal (refactor)** — reworked modal, later reworked again with improved form layout.
- **Edit and delete invoices** — `editInvoice` / `deleteInvoice` exposed through the database layer.
- **Negative invoice lines** — quantities can be negative.
- **Invoice line deletion** — fixed removing lines from an invoice.
- **References** — multiple delivery-note references per document, mapped through pivot tables.
- **Quick creation shortcuts** — create customers and references directly from the creation modals.
- **Anti-accidental-cancel guards** — confirmation flows protect against losing unsaved work.

## Budgets

- **Budget management** — full implementation of budgets with the same line-item model as invoices.
- **Promote budget → invoice** — convert budgets into invoices (repeatable; a promoted budget is marked in the UI).
- **Budget templates** — dedicated Word/Excel templates for budgets.
- **Home dashboard budgets** — budgets shown and accessible from the home page.

## Dashboard / Home

- **Quick actions** — create invoices and customers directly from the home page.
- **Recent items previews** — preview modals for recent invoices and customers.
- **Clickable home items** and counters (next invoice/budget numbering, business logo).
- **Budgets on the dashboard** and gating adjustments in the header.

## Search & Selector

- **Client search integration**.
- **Reusable Selector component** — generic, keyboard-navigable selector (`Select`-style) used for customer selection in invoice creation/editing, later extended to arbitrary types.
- **Animation-synced modals and selector** — selectors respond to `animationend` for smoother UX.

## Export

- **Excel (.xlsx) export** — export invoices to Excel with final template (units, no underline).
- **Word (.docx) export** — export to Word with company data.
- **PDF export** — full PDF export (pdfkit) with:
  - invoice/budget info and customer data repeated on every page,
  - **settings section for controllable offsets**,
  - fixups for line deletion and layout.
- **Persistent export path** — last used export directory is remembered.
- **Error handling** — exporting provides feedback on failures.
- **Bundled templates** — templates packaged with the app assets.

## Settings

- **Settings page** with real data (next invoice / next budget numbering).
- **Document formats** section reflecting actual configuration.
- **Export PDF settings** — configurable offsets.
- **Min/max window size** constraints and visual fixes.

## Packaging & polish

- **Installer** — app shipped as a Windows installer built with Electron Forge.
- **App name and icon** — branding, spelling fixes.
- **DevTools removed** for the packaged app.
- **Visual polish** — modals animations, confirm dialogs, alignment, and minor layout tweaks.