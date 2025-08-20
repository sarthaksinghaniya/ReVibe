<div align="center">

# ReVibe – E‑Waste Innovation Platform ♻️✨

Turning household e‑waste into low‑cost, authentic projects for education, energy, art, and assistive solutions.

</div>

---

## 📖 Overview

ReVibe helps communities transform common e‑waste into useful, creative, and affordable innovations. It provides:

- A normalized relational database of materials, projects, tools, tags, and step‑by‑step instructions.
- ETL pipelines to safely ingest CSV datasets into SQLite or Postgres with foreign key enforcement.
- Query utilities to suggest projects by available materials, search by criteria, and fetch full project details.
- A React front‑end (Vite) to explore and present ideas. An Express API backend is planned.

## 🌍 Goals & Purpose

- Empower learners, makers, and educators with practical, low‑cost project ideas from e‑waste.
- Promote sustainability and circular economy principles via reuse and innovation.
- Provide a robust, open data model and tooling that others can extend or localize.

## 📂 Repository Structure

```
E-waste/revibe/
├─ .env.example
├─ .gitignore
├─ README.md              # You are here
├─ eslint.config.js       # ESLint flat config (ESLint v9)
├─ index.html
├─ package.json
├─ package-lock.json
├─ vite.config.js
├─ public/
│  └─ favicon.svg
├─ src/
│  ├─ App.css
│  ├─ App.jsx             # UI (branding: ReVibe)
│  ├─ main.jsx
│  └─ assets/
└─ db/
   ├─ README.md           # DB usage docs (schema, ETL, CLI)
   ├─ schema.sqlite.sql   # SQLite schema
   ├─ schema.postgres.sql # Postgres schema
   ├─ queries.js          # Query helpers + CLI
   └─ etl/
      └─ import.js        # CSV ETL importer (SQLite/Postgres)
```

## 📊 Data & Database

Schemas are normalized with strict foreign keys and meaningful constraints.

- Reference tables: `materials`, `tools`, `tags`
- Core: `projects` (title, description, difficulty, cost, sdg_target, duration_minutes)
- Junctions: `project_materials`, `project_tools`, `project_tags`
- Steps: ordered `steps` per project with unique `(project_id, step_number)`

Files:

- SQLite schema: `db/schema.sqlite.sql`
- Postgres schema: `db/schema.postgres.sql`

### ETL (CSV → DB)

- Script: `db/etl/import.js`
- Client: SQLite or Postgres (via `.env` or `--client` flag)
- Behavior: transactional import, FK enforcement, idempotent upserts by name/title
- Difficulty normalization: `easy|medium|hard`

Expected CSVs (place in `./data/`):

- `materials.csv`: id?, name, category?, notes?
- `tools.csv`: id?, name, notes?
- `tags.csv`: id?, name, type?
- `projects.csv`: id?, title, description?, difficulty?, cost?, sdg_target?, duration_minutes?
- `project_materials.csv`: project_id?|project_title, material_id?|material_name, quantity?, unit?
- `project_tools.csv`: project_id?|project_title, tool_id?|tool_name, notes?
- `project_tags.csv`: project_id?|project_title, tag_id?|tag_name
- `steps.csv`: project_id?|project_title, step_number, instruction, duration_minutes?

### Queries

Module/CLI: `db/queries.js`

- Suggest projects by materials: returns projects where required materials ⊆ available
- Search projects: filter by `difficulty`, `maxCost`, `sdg_target`
- Project details: steps, materials, tools, and tags for a selected project

## 🛠 Tech Stack

- Front‑end: React + Vite (HMR) — `src/`
- Backend (planned): Express/Node.js (API to wrap queries)
- Database: SQLite (dev) and Postgres (prod‑ready)
- ETL: Node.js, `csv-parse`, `dotenv`, `better-sqlite3`, `pg`
- Tooling: ESLint (Flat config v9), npm scripts, Vite

## 🚀 Setup & Usage

1) Clone and enter the project folder

```bash
git clone <your-fork-or-repo-url>
cd E-waste/revibe
```

2) Install dependencies

```bash
npm install
```

3) Configure environment

```bash
copy .env.example .env   # Windows
# or: cp .env.example .env
```

Edit `.env`:

- SQLite (default)
  - `DB_CLIENT=sqlite`
  - `DB_SQLITE_PATH=./db/innovation.sqlite3`
- Postgres
  - `DB_CLIENT=postgres`
  - `DATABASE_URL=postgres://user:pass@host:5432/dbname`

4) Prepare data and import

Place CSV files in `E-waste/revibe/data/` (create if missing), then run one of:

```bash
npm run db:import:sqlite -- --data ./data
# or
npm run db:import:pg -- --data ./data
```

5) Run queries (CLI)

```bash
# Suggest by materials
npm run db:query -- suggest --materials "plastic bottle, cardboard"

# Search by criteria
npm run db:query -- search --difficulty easy --maxCost 200 --sdg SDG12

# Get project details
npm run db:query -- details --project "Mini greenhouse for seedlings"
```

6) Start the dev server (UI)

```bash
npm run dev
# open the URL Vite prints (usually http://localhost:5173)
```

## 📜 NPM Scripts

| Script               | Description                                              |
|----------------------|----------------------------------------------------------|
| `dev`                | Start Vite dev server with HMR                           |
| `build`              | Production build (Vite)                                  |
| `preview`            | Preview built app                                        |
| `lint`               | Run ESLint (flat config)                                 |
| `db:import:sqlite`   | Import CSVs into SQLite database                         |
| `db:import:pg`       | Import CSVs into Postgres database                       |
| `db:query`           | Run query CLI (suggest/search/details)                   |

## 👨‍💻 Developer

- **Name:** Sarthak Singhaniya  
- **GitHub:** [@sarthaksinghaniya](https://github.com/sarthaksinghaniya)  
- **Background:** B.Tech CSE (AI), AIML Engineer, Python/JS Developer

## 🤝 Contributing

Contributions are welcome! Please:

1. Fork the repo and create a feature branch.
2. Keep changes focused and add/update docs/tests where relevant.
3. Follow ESLint rules (`npm run lint`).
4. Open a PR with a clear description and screenshots/logs if applicable.

For data additions, ensure CSVs match the expected columns and pass ETL import locally.

## 📜 License

This project is licensed under the **MIT License**. See the LICENSE file (or include the standard MIT text in your fork).

## 🌟 Future Enhancements

- Express API endpoints for search/suggest/details
- UI components to browse/import datasets and visualize coverage by materials
- Advanced ranking (cost/difficulty/SDG alignment scoring)
- Internationalization and localized descriptions
- Data validation reports during ETL (schema + semantic checks)
- Optional auth for managing curated project libraries

---

If you build something cool with ReVibe, share it! 🌱
