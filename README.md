# Finance Dashboard UI

**Screening Assignment — Frontend Developer Intern**  
**Zorvyn FinTech Pvt. Ltd.**  
**Submitted by:** Yasaswini Avvaru

---

## Project Overview

A clean, interactive finance dashboard interface built with **plain HTML, CSS, and JavaScript** (with Chart.js via CDN). This project demonstrates core frontend competencies: component design, data visualization, role-based UI behavior, state management, responsiveness, and polished UX.

No frameworks. No build tools. Every line of code is handwritten.

---

## Tech Stack

| Technology | Purpose |
|---|---|
| HTML5 | Page structure & semantic markup |
| CSS3 | Styling, dark mode (CSS variables), responsive layout, animations |
| Vanilla JavaScript | State management, DOM manipulation, event handling |
| Chart.js (CDN) | Line chart & Doughnut chart visualization |
| Google Material Icons | Insight card icons & export button icon |

---

## How to Run Locally

1. Clone or download this repository.
2. Open `index.html` directly in a browser, **or** serve via a local HTTP server for Chart.js CDN to load:
   ```bash
   npx http-server -p 8080
   ```
3. Navigate to `http://localhost:8080` in your browser.

> **Note:** Opening via `file://` protocol may block Chart.js CDN loading in some browsers. Using a local server is recommended.

---

## Features

### Dashboard Overview
- **Summary Cards** — Total Balance, Income, Expenses (computed from data, formatted as ₹ with Indian locale)
- **Line Chart** — Monthly Balance Trend (Balance, Income, Expenses over time)
- **Doughnut Chart** — Spending by Category (with percentage tooltips)

### Transactions
- Sortable, searchable, filterable table with 25 mock entries
- **Search** by description or category (live filtering)
- **Filter** by type (Income/Expense) and category
- **Dynamic Category Filtering** — category dropdown updates based on selected type (Income shows only Salary, Freelance; Expense shows only expense categories)
- **Sort** by date or amount (ascending/descending)
- **Export CSV** — exports currently filtered transactions as a downloadable CSV file
- **Empty State** — friendly "No transactions found" message with icon when filters yield no results; table is hidden

### Role-Based UI
- **Viewer** (default): Read-only access to dashboard, charts, and transactions
- **Admin**: Can add and edit transactions via a modal form
- Switch roles using the dropdown in the top navbar
- Role is persisted across page refreshes

### Insights
- **Highest Spending Category** — with icon, amount, and transaction count
- **Month-over-Month** — compares current vs previous month's expenses with trend icon (↑/↓)
- **Income vs Expense Ratio** — with savings rate percentage
- **Visual Polish** — color-coded accent stripes (green/red), Material Icons, sentiment-based coloring

### Dark Mode
- Toggle via navbar button (🌙/☀️)
- Uses CSS custom properties — all colors swap via `.dark-mode` class
- Preference persisted in localStorage

### Data Persistence
- Transactions, role, and dark mode preference saved to `localStorage`
- Loads automatically on page refresh
- Falls back to mock data if no saved state exists

### Currency Formatting
- All amounts displayed using **₹** (Indian Rupee) symbol
- Formatted with `toLocaleString('en-IN')` for proper Indian number formatting (e.g., ₹12,088.00)

### Responsive Design
- **Desktop**: Fixed sidebar, 3-column grid layouts
- **Tablet (≤1024px)**: Charts stack vertically, insights use 2-column grid
- **Mobile (≤768px)**: Sidebar collapses to hamburger menu with overlay tap-to-close, cards stack vertically, table scrolls horizontally, filters go full-width
- **Small Mobile (≤480px)**: Further reduced font sizes and gaps

### Animations & Transitions
- Hover effects on cards (translateY), buttons, chart containers, and edit buttons
- Fade-in animation on section switches
- Staggered card entrance animation
- Modal entrance animation (scale + fade)
- Smooth 0.25s transitions throughout

---

## Project Structure

```
finance-dashboard/
├── index.html          ← Main HTML shell
├── style.css           ← All styling (responsive, dark mode, animations)
├── app.js              ← App entry point, state, event listeners, localStorage, CSV export
├── data.js             ← Mock transactions data (25 entries)
├── charts.js           ← Chart.js initialization & update logic
├── transactions.js     ← Render, filter, sort, search, dynamic category filtering
├── insights.js         ← Insights computation & polished rendering
└── README.md           ← This file
```

---

## State Management

A single `appState` object serves as the central source of truth:

```javascript
const appState = {
  transactions: [],     // All transactions (source of truth)
  filtered: [],         // Currently displayed after filter/search/sort
  role: 'viewer',       // 'viewer' or 'admin'
  filter: { type, category, search },
  sort: 'date-desc',
  darkMode: false,
  editingId: null       // ID of transaction being edited
};
```

When any state changes (role switch, filter, add/edit transaction), a central `render()` function is called which updates all DOM elements based on the current state. This keeps logic **predictable** and easy to trace.

State is persisted to `localStorage` on every mutation and loaded on page initialization.

---

## Role-Based UI — Demo Steps

1. **Load the page** → Default role is "Viewer"
2. In the top navbar, change the **Role** dropdown to **"Admin"**
3. Navigate to **Transactions** → you'll see:
   - A **"+ Add Transaction"** button appears
   - **"Edit"** buttons appear on every row
4. Click **"+ Add Transaction"** → fill out the form → Save
5. The new transaction immediately appears in the table and updates summary cards + charts
6. Switch back to **Viewer** → admin controls disappear

---

## Dynamic Category Filtering — Demo Steps

1. Navigate to **Transactions**
2. Select **"Income"** from the Type filter dropdown
3. The Category dropdown updates to show only: **Freelance**, **Salary**
4. Select **"Expense"** from the Type filter
5. The Category dropdown updates to show only expense categories
6. If a category was previously selected but is no longer valid, it resets to "All Categories"

---

## Export CSV — Demo Steps

1. Navigate to **Transactions**
2. Optionally apply filters (type, category, search)
3. Click **"Export CSV"** button
4. A CSV file downloads with columns: Date, Description, Category, Type, Amount
5. Only currently filtered/visible transactions are exported

---

## Mock Data

25 transactions spread across **February, March, and April 2026**, covering 7 categories:

| Category | Type |
|---|---|
| Salary | Income |
| Freelance | Income |
| Food & Dining | Expense |
| Rent & Utilities | Expense |
| Shopping | Expense |
| Entertainment | Expense |
| Healthcare | Expense |

---

## Design Decisions

- **Color Palette**: Teal `#0F766E` as primary accent for a professional fintech feel
- **Typography**: Inter (Google Fonts) for modern, clean readability
- **Layout**: Fixed sidebar (desktop) collapsing to hamburger menu (mobile)
- **Dark Mode**: Implemented via CSS custom properties — all colors swap via `.dark-mode` class
- **Currency**: ₹ (Indian Rupee) with `en-IN` locale formatting throughout
- **Icons**: Google Material Icons Outlined for insight cards and export button
- **No framework**: Intentional choice to demonstrate raw JavaScript proficiency

---

## Edge Cases Handled

- **Empty state**: Friendly message when no transactions match filters (table hidden, icon + text shown)
- **Invalid category on type switch**: Category filter resets to "All" if the previously selected category doesn't exist in the new type
- **localStorage corruption**: Gracefully falls back to mock data if saved state is corrupted or malformed
- **Zero expenses**: Insights show ∞ ratio and 100% savings rate when no expenses exist
- **Form validation**: Prevents submission with empty fields, negative amounts, or zero values
- **CSV escaping**: Description and category values with special characters are properly quoted

---

## Known Limitations

- Data is not persisted to a backend — changes are stored in browser localStorage only
- Chart.js requires CDN access (won't load via `file://` protocol in some browsers)
- No pagination — all 25 transactions render at once (suitable for this data size)
- No delete functionality (only add/edit)

---

## Deployed Link

*GitHub Pages / Netlify — to be added after deployment*

---

**Built with care by Yasaswini Avvaru**
