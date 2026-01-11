# Money Management App  

![React](https://img.shields.io/badge/React-19.0.0-blue?logo=react) ![TailwindCSS](https://img.shields.io/badge/TailwindCSS-4.0.6-38B2AC?logo=tailwindcss) ![License](https://img.shields.io/badge/License-MIT-green) ![Version](https://img.shields.io/badge/Version-0.1.0-lightgrey)  

**A sleek, AI‑enhanced personal finance dashboard built with React, TailwindCSS and Recharts.**  

---  

## Table of Contents  

- [Overview](#overview)  
- [Features](#features)  
- [Tech Stack](#tech-stack)  
- [Architecture](#architecture)  
- [Getting Started](#getting-started)  
  - [Prerequisites](#prerequisites)  
  - [Installation](#installation)  
  - [Configuration](#configuration)  
  - [Running the App](#running-the-app)  
- [Usage](#usage)  
- [Development](#development)  
- [Testing](#testing)  
- [Deployment](#deployment)  
- [API – AI‑Assisted Suggestions](#api-%E2%80%93-ai%E2%80%93assisted-suggestions)  
- [Contributing](#contributing)  
- [Roadmap](#roadmap)  
- [License & Credits](#license--credits)  

---  

## Overview  

Money Management App is a responsive web application that helps users track income, expenses, and savings goals in real‑time. Leveraging **Recharts** for interactive visualisations and **Google Generative AI** for smart budgeting suggestions, the app provides a modern, intuitive interface built with **React 19** and **TailwindCSS**.  

- **Who is it for?**  
  - Individuals who want a quick overview of their cash flow.  
  - Developers looking for a clean, component‑driven starter for finance‑related projects.  

- **Current status:** Early development (v0.1.0). Core UI and charting are functional; AI‑driven insights are in beta.  

---  

## Features  

| Feature | Description | Status |
|---------|-------------|--------|
| **Dashboard** | Overview cards for total balance, monthly income, expenses, and savings. | ✅ Stable |
| **Transaction List** | Add, edit, delete transactions with categories & tags. | ✅ Stable |
| **Interactive Charts** | Line & pie charts (via Recharts) showing cash‑flow trends. | ✅ Stable |
| **Budget Planner** | Set monthly budgets per category and get visual alerts. | ✅ Stable |
| **AI‑Assisted Suggestions** | Uses `@google/generative-ai` to propose budgeting adjustments based on spending patterns. | 🧪 Beta |
| **Responsive Design** | Mobile‑first layout powered by TailwindCSS. | ✅ Stable |
| **Dark / Light Theme** | Toggleable UI theme using Shadcn UI primitives. | ✅ Stable |
| **Export / Import** | CSV export of transactions and JSON import for backup. | 🧪 Beta |
| **Local Storage Persistence** | Data persisted in the browser’s `localStorage` (no backend required). | ✅ Stable |

---  

## Tech Stack  

| Layer | Technology | Reason |
|-------|------------|--------|
| **Framework** | React 19 (via `react-scripts`) | Modern component model, fast hot‑reloading |
| **Styling** | TailwindCSS 4 + Shadcn UI | Utility‑first CSS + pre‑built accessible components |
| **Charts** | Recharts 2.15 | Declarative chart components for React |
| **Icons** | Lucide‑React | Consistent, open‑source icon set |
| **AI** | `@google/generative-ai` | Generates budgeting insights and expense categorisation |
| **Build Tool** | Create‑React‑App (react‑scripts) | Zero‑config development & production builds |
| **Testing** | Jest + React Testing Library (via CRA) | Unit & integration testing out‑of‑the‑box |
| **Version Control** | Git | Standard source‑code management |
| **Package Manager** | npm (v9+) | Handles dependencies |

---  

## Architecture  

```
money-management-app/
├─ public/                 # Static assets (index.html, favicons, manifest)
├─ src/
│  ├─ App.js               # Root component – layout, routing, theme provider
│  ├─ App.css              # Global styles (Tailwind imports)
│  ├─ index.js             # Entry point – renders <App />
│  ├─ index.css            # Tailwind base + custom utilities
│  ├─ components/          # Re‑usable UI components (DashboardCard, TransactionForm, etc.)
│  ├─ hooks/               # Custom React hooks (useTransactions, useAIInsights)
│  ├─ services/            # Wrapper around localStorage & Google AI client
│  ├─ utils/               # Helper functions (formatCurrency, calculateTotals)
│  └─ assets/              # Images, SVG icons
├─ .gitignore
├─ package.json
└─ README.md
```

* **Entry point (`index.js`)** – creates the React root and mounts `<App />`.  
* **State management** – simple `useState`/`useReducer` hooks; data persisted to `localStorage`.  
* **AI Service** – `services/ai.js` initialises the Generative AI client with an API key from `.env`.  

---  

## Getting Started  

### Prerequisites  

| Tool | Minimum version |
|------|-----------------|
| **Node.js** | 18.x |
| **npm** | 9.x (comes with Node) |
| **Git** | any recent version |

> **Note** – The AI features require a **Google Generative AI API key**. Sign up at the [Google AI Studio](https://aistudio.google.com/) and enable the Generative AI API.

### Installation  

```bash
# 1️⃣ Clone the repository
git clone https://github.com/xevrion/money-manageuuu.git
cd money-manageuuu/money-management-app

# 2️⃣ Install dependencies
npm ci   # installs exact versions from package-lock.json
```

### Configuration  

Create a `.env.local` file in the project root (same level as `package.json`):

```dotenv
# .env.local
REACT_APP_GEMINI_API_KEY=YOUR_GOOGLE_GENERATIVE_AI_KEY
```

*The `.env*` files are listed in `.gitignore` (the recent commit added this rule), so they will **not** be committed to the repository.*  

> The `REACT_APP_` prefix is required for CRA to expose the variable to the browser.

### Running the App  

```bash
npm start
```

The development server starts at **http://localhost:3000** and hot‑reloads on file changes.  

---  

## Usage  

### Adding a Transaction  

```tsx
import { useTransactions } from '../hooks/useTransactions';
import { TransactionForm } from '../components/TransactionForm';

function NewTransaction() {
  const { addTransaction } = useTransactions();

  const handleSubmit = (data) => {
    addTransaction(data);
  };

  return <TransactionForm onSubmit={handleSubmit} />;
}
```

### Viewing the Dashboard  

The default route (`/`) renders the `Dashboard` component, which displays:

- **Balance Card** – total cash on hand.  
- **Income / Expense Chart** – line chart of monthly cash‑flow.  
- **Category Breakdown** – pie chart of spending per category.  

```tsx
import { Dashboard } from '../components/Dashboard';

export default function Home() {
  return (
    <main className="p-4">
      <Dashboard />
    </main>
  );
}
```

### AI‑Assisted Budget Suggestion  

```tsx
import { useAIInsights } from '../hooks/useAIInsights';

function BudgetSuggestion() {
  const { suggestion, loading, error } = useAIInsights();

  if (loading) return <p>Generating suggestion…</p>;
  if (error) return <p>Unable to fetch AI insight.</p>;

  return (
    <div className="bg-gray-50 p-4 rounded">
      <h3 className="font-semibold">AI Budget Tip</h3>
      <p>{suggestion}</p>
    </div>
  );
}
```

The hook internally calls:

```js
import { GoogleGenerativeAI } from '@google/generative-ai';

const genAI = new GoogleGenerativeAI(process.env.REACT_APP_GEMINI_API_KEY);
```

---  

## Development  

| Task | Command |
|------|---------|
| **Start dev server** | `npm start` |
| **Run tests** | `npm test` |
| **Build for production** | `npm run build` |
| **Eject (advanced)** | `npm run eject` |

### Code Style  

- **Prettier** (via CRA) – run `npm run format` if you add a script.  
- **ESLint** – follows `react-app` and `react-app/jest` configs.  

### Debugging Tips  

- Open the browser devtools → **React** tab to inspect component state.  
- Use `console.log` inside custom hooks (`useTransactions`, `useAIInsights`) to trace data flow.  

---  

## Testing  

The project ships with the default CRA test setup (`src/App.test.js`). Add new tests alongside components:

```tsx
// Example: src/components/__tests__/Dashboard.test.tsx
import { render, screen } from '@testing-library/react';
import { Dashboard } from '../Dashboard';

test('renders balance card', () => {
  render(<Dashboard />);
  expect(screen.getByText(/balance/i)).toBeInTheDocument();
});
```

Run `npm test` to launch the watch mode.  

---  

## Deployment  

### Static Hosting (Netlify / Vercel / GitHub Pages)  

```bash
npm run build   # creates ./build
# Deploy the ./build folder to your preferred static host
```

### Docker (optional)  

```dockerfile
# Dockerfile
FROM node:20-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

FROM nginx:stable-alpine
COPY --from=builder /app/build /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

Build & run:

```bash
docker build -t money-management-app .
docker run -p 8080:80 money-management-app
```

Visit **http://localhost:8080**.  

---  

## API – AI‑Assisted Suggestions  

The app does **not** expose a public REST API, but it communicates with Google’s Generative AI service internally.

| Endpoint (internal) | Purpose | Request | Response |
|----------------------|---------|---------|----------|
| `POST /ai/suggest-budget` | Generate a budgeting tip based on recent transactions. | `{ transactions: Transaction[] }` | `{ suggestion: string }` |
| `POST /ai/categorize` | Auto‑assign a category to a free‑text expense description. | `{ description: string }` | `{ category: string, confidence: number }` |

**Authentication** – The client sends the API key via the `Authorization: Bearer <key>` header; the key is stored in `process.env.REACT_APP_GEMINI_API_KEY`.  

---  

## Contributing  

We welcome contributions! Follow these steps:

1. **Fork** the repository.  
2. **Create a feature branch**: `git checkout -b feat/your-feature`.  
3. **Install dependencies** (`npm ci`).  
4. **Make your changes** – ensure they pass linting and tests.  
5. **Write tests** for new functionality.  
6. **Commit** with a clear message (`git commit -m "feat: add XYZ"`).  
7. **Push** to your fork and open a **Pull Request** against `main`.  

### Pull Request Checklist  

- [ ] Code follows the existing style (Prettier + ESLint).  
- [ ] New/updated components have unit tests with ≥80% coverage.  
- [ ] Documentation (README, inline comments) is updated if needed.  
- [ ] No linting or test failures (`npm test` passes).  

---  

## Roadmap  

| Milestone | Target Release | Planned Features |
|-----------|----------------|------------------|
| **v0.2.0** | Q2 2026 | Multi‑currency support, recurring transactions, improved AI model (Gemini 1.5). |
| **v1.0.0** | Q4 2026 | User authentication (OAuth), backend sync (Firebase/ Supabase), mobile PWA enhancements. |
| **v1.1.0** | 2027 | Export to PDF, budgeting templates, community theme marketplace. |

---  

## License & Credits  

**License:** MIT © 2026 **xevrion**  

```
Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions…

[Full MIT license text in LICENSE file]
```

### Acknowledgments  

- **Create React App** – scaffolding and build tooling.  
- **Shadcn UI** – accessible component primitives.  
- **Lucide** – open‑source icons.  
- **Google Generative AI** – powering the budgeting insights.  

### Contributors  

- **xevrion** – project creator & primary maintainer.  

---  

*Happy budgeting!*