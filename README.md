# Research OS - Antigravity Experiment

> Complete standalone experiment repository for the Antigravity Workstation architecture.

## 🏗️ Architecture Overview

This is a **completely separate repository** from the main Research OS, implementing the Antigravity Workstation architecture with:

- **4-tab Workstation** (Overview, Financial Statements, Scenarios, Valuation & Forensic)
- **Multi-modal Document Ingestion** (Excel, PDF, Image, Link, Text)
- **Workbench with Notes & Attachments** per card
- **Reactive Scenario Modeling** with trajectory charts & margin bridges
- **Source of Truth Drawer** with citation audit trail
- **No Authentication** - fully open for testing

---

## 📁 Project Structure

```
Research-OS-Antigravity/
├── research-os-frontend/          # Next.js 15 App Router (port 3000)
│   ├── src/app/
│   │   ├── company-antigravity/[symbol]/   # Antigravity Workstation pages
│   │   │   ├── layout.tsx       # Workstation header with actions
│   │   │   └── page.tsx         # Workstation canvas + tabs
│   │   ├── api/workstation/     # BFF proxy to Python backend
│   │   └── ... (other pages)
│   ├── src/components/
│   │   ├── workstation/         # Workstation components
│   │   │   ├── WorkstationCanvas.tsx
│   │   │   ├── WorkstationTabNav.tsx
│   │   │   ├── WorkbenchCard.tsx
│   │   │   ├── WorkstationWidgetRenderer.tsx
│   │   │   ├── WidgetCatalogModal.tsx
│   │   │   ├── workstation-widget-catalog.ts
│   │   │   └── widgets/         # 25+ workstation widgets
│   │   ├── financials/          # Financial statement components
│   │   │   ├── MultiModalUploader.tsx
│   │   │   ├── FinancialStatementsWorkstation.tsx
│   │   │   ├── FinancialStatementTable.tsx
│   │   │   └── SourceOfTruthDrawer.tsx
│   │   └── scenarios/           # Scenario modeling components
│   │       ├── ScenarioWorkstation.tsx
│   │       ├── DriverInputs.tsx
│   │       ├── TrajectoryChart.tsx
│   │       └── MarginBridgeChart.tsx
│   ├── src/lib/
│   │   ├── workstation-store.ts    # Zustand store for workstation state
│   │   ├── scenario-store.ts       # Zustand store for scenarios
│   │   └── ...
│   └── tests/                     # 266 tests (14 test files)
│
├── math-engine/                    # Python FastAPI (port 8000)
│   ├── api/main.py                 # 18 calculation endpoints
│   ├── src/math_engine/            # 18 engines
│   │   ├── valuation/              # DCF, Relative, SOTP, Banking, Excess Return
│   │   ├── forensics/              # Beneish, Altman, Piotroski
│   │   ├── portfolio/              # Portfolio Analytics, Corporate Actions
│   │   ├── screening/              # Screening Engine
│   │   └── ...
│   └── tests/                      # 76 tests
│
├── data-ingestion/                 # Python FastAPI (port 8001)
│   ├── api/
│   │   ├── main.py                 # Main FastAPI app
│   │   ├── workstation_router.py   # Workstation persistence endpoints
│   │   └── data_ingestion/         # Ingestion endpoints
│   ├── src/data_ingestion/
│   │   ├── extractors/             # Multi-modal extractors
│   │   │   ├── excel.py            # Screener.in/BSE Excel
│   │   │   ├── pdf.py              # PyMuPDF
│   │   │   ├── image.py            # OCR/Image extraction
│   │   │   ├── link.py             # Web link extraction
│   │   │   ├── legal.py            # Legal documents
│   │   │   └── unstructured.py     # General unstructured
│   │   ├── models/
│   │   │   ├── db_workstation.py   # PostgreSQL persistence
│   │   │   ├── store_workstation.py # In-memory + PG cache
│   │   │   ├── taxonomy.py         # Line item canonicalization
│   │   │   └── statements.py       # Statement models
│   │   └── ...
│   └── tests/                      # 54 tests
│
├── postgres/init/
│   ├── 01_schema.sql               # Main schema (30 tables)
│   └── 02_workstation_schema.sql   # Workstation tables (4 tables)
│
└── docker-compose.yml              # Local development stack
```

---

## 🚀 Quick Start

### Prerequisites
- Python 3.11+
- Node.js 20+
- PostgreSQL 15+ (optional - falls back to in-memory)

### 1. Start Python Services
```bash
# Terminal 1: Math Engine
cd math-engine
python -m pip install -e ".[dev,test]"
python -m uvicorn api.main:app --host 0.0.0.0 --port 8000

# Terminal 2: Data Ingestion
cd data-ingestion
python -m pip install -e ".[dev,test]"
python -m uvicorn api.main:app --host 0.0.0.0 --port 8001
```

### 2. Start Frontend
```bash
# Terminal 3: Frontend
cd research-os-frontend
npm install --legacy-peer-deps
npm run dev
```

### 3. Access the Application
- **Main Dashboard**: http://localhost:3000
- **Antigravity Workstation**: http://localhost:3000/company-antigravity/RELIANCE
- **Math Engine API**: http://localhost:8000
- **Data Ingestion API**: http://localhost:8001

---

## 🔬 Test Results Summary

| Component | Tests | Status |
|-----------|-------|--------|
| **Math Engine** | 76 | ✅ All Pass |
| **Data Ingestion** | 54 | ✅ All Pass |
| **Frontend** | 266 | ✅ All Pass |
| **TypeScript** | - | ✅ 0 Errors |
| **Build** | - | ✅ Success |

---

## 📊 API Endpoints

### Math Engine (port 8000)
| Endpoint | Method | Description |
|----------|--------|-------------|
| `/health` | GET | Health check |
| `/dcf` | POST | DCF valuation |
| `/relative` | POST | Relative valuation |
| `/sotp` | POST | Sum-of-the-parts |
| `/banking` | POST | Banking valuation |
| `/excess-return` | POST | Excess return model |
| `/beneish` | POST | Beneish M-Score |
| `/altman` | POST | Altman Z-Score |
| `/piotroski` | POST | Piotroski F-Score |
| `/portfolio` | POST | Portfolio analytics |
| `/ratio-analysis` | POST | Ratio analysis (40+ ratios) |
| `/turnaround` | POST | Turnaround detection |
| `/hedging` | POST | Hedging mechanics |
| `/risk-score` | POST | Risk scoring (SEBI LODR) |
| `/common-size` | POST | Common-size statements |
| `/revenue-projection` | POST | Revenue projection |
| `/concall-consistency` | POST | Concall consistency |
| `/corporate-actions` | POST | Corporate actions adj. |
| `/prices` | GET | Mock price data |

### Data Ingestion (port 8001)
| Endpoint | Method | Description |
|----------|--------|-------------|
| `/health` | GET | Health check |
| `/ingest/excel` | POST | Upload Excel (Screener.in/BSE) |
| `/ingest/pdf` | POST | Upload PDF |
| `/ingest/image` | POST | Upload Image (OCR) |
| `/ingest/link` | POST | Extract from URL |
| `/ingest/text` | POST | Ingest raw text |
| `/statements/{symbol}` | GET | Get statements with citations |
| `/statements/{symbol}/citations/{id}` | GET | Get citation source |
| `/api/workstation/layout/{symbol}` | GET/POST | Workstation layout persistence |
| `/api/workstation/notes/{symbol}` | GET/POST | Card notes CRUD |
| `/api/workstation/attachments/{symbol}` | GET/POST | File attachments |

---

## 🎯 Key Features Implemented

### 1. Antigravity Workstation (`/company-antigravity/[symbol]`)
- **4 Tabs**: Overview, Financial Statements, Scenarios, Valuation & Forensic
- **Drag-drop tabs** with reordering
- **Add/Remove/Rename custom tabs**
- **Widget catalog** with 25+ widget types
- **react-grid-layout** responsive canvas
- **LocalStorage + PostgreSQL** persistence
- **Real-time sync status** indicator

### 2. Multi-Modal Document Ingestion
| Format | Extractor | Features |
|--------|-----------|----------|
| Excel/CSV | `excel.py` | Screener.in, BSE/NSE, multi-year, citations |
| PDF | `pdf.py` | PyMuPDF, table extraction, page-level citations |
| Image | `image.py` | OCR, preprocessing, region extraction |
| Web Link | `link.py` | HTML extraction, Screener.in parsing |
| Text | `unstructured.py` | Concall speaker diarization, chunking |

### 3. Financial Statements Workstation
- **Multi-year P&L, BS, CF** (11+ years)
- **INR Crores** standard unit
- **Source of Truth Drawer** - as-reported line items
- **Citation audit trail** - every cell traceable to source
- **Common-size toggle** - % of revenue/assets

### 4. Scenario Modeling
- **Reactive 5-year simulation**
- **Driver inputs** (volume, price, margin, capex, WC)
- **Trajectory charts** - revenue, EBITDA, FCF paths
- **Margin bridge** - waterfall decomposition
- **Segment projections** - per-business-unit

### 5. Workbench (Notes & Attachments)
- **Per-card notes** with tags, metadata
- **Multi-modal attachments** (PDF, Excel, images)
- **Drag-drop upload** with progress
- **Download & delete** capabilities

---

## 🗄️ Database Schema

### Workstation Tables (02_workstation_schema.sql)
| Table | Purpose |
|-------|---------|
| `user_workstation_layouts` | Tab + widget grid configs per user/symbol |
| `card_notes` | Qualitative notes per card/symbol |
| `card_attachments` | File metadata + binary content (BYTEA) |

### Main Schema (01_schema.sql)
30 tables covering: companies, financials, documents, vectors, users, portfolios, etc.

---

## 🔧 Configuration

### Environment Variables

**Frontend** (`.env.local`):
```bash
NEXT_PUBLIC_MATH_ENGINE_URL=http://localhost:8000
NEXT_PUBLIC_DATA_INGESTION_URL=http://localhost:8001
NEXT_PUBLIC_APP_URL=http://localhost:3000
```

**Math Engine** (`.env`):
```bash
DATABASE_URL=postgresql://research:research_local_dev@localhost:5433/research_os_ag
```

**Data Ingestion** (`.env`):
```bash
DATABASE_URL=postgresql://research:research_local_dev@localhost:5433/research_os_ag
QDRANT_URL=http://localhost:6334
```

---

## 🧪 Running Tests

```bash
# All tests
cd math-engine && python -m pytest tests/ -v
cd data-ingestion && python -m pytest tests/ -v
cd research-os-frontend && npm test

# Specific test files
cd data-ingestion && python -m pytest tests/test_multi_modal_ingestion.py -v
cd math-engine && python -m pytest tests/test_adversarial_endpoints.py -v
cd research-os-frontend && npm test -- tests/workstation-workbench.test.ts
```

---

## 📝 Notes

### No Authentication
This experiment runs **without authentication** for easy testing:
- `middleware.ts` is a simple pass-through
- No NextAuth, no sign-in page
- All routes accessible directly

### PostgreSQL Optional
Services gracefully fall back to **in-memory storage** when PostgreSQL is unavailable:
- Workstation layouts cached in localStorage + memory
- Notes/attachments stored in memory
- Zero-config development

### Separate from Main Repo
This is a **completely independent repository**:
- Different ports (3000, 8000, 8001 vs 3000, 8000, 8001 on main)
- Different database (`research_os_ag` vs `research_os`)
- Different architecture (Workstation vs 10-tab Dashboard)
- Can run side-by-side with main Research OS

---

## 📈 Comparison: Main vs Antigravity

| Feature | Main Research OS | Antigravity Experiment |
|---------|------------------|------------------------|
| **Company Page** | 10 tabs, 86 widgets | 4 workstation tabs, 25 widgets |
| **Architecture** | Widget-per-tab | Workbench + Canvas |
| **Ingestion** | Basic PDF | Multi-modal (Excel, PDF, Image, Link, Text) |
| **Statements** | Mock data | Real extraction + citations |
| **Scenarios** | Simple DCF | Reactive 5-year + margin bridge |
| **Notes/Attachments** | Not implemented | Full workbench |
| **Auth** | Disabled | Disabled |
| **Database** | research_os | research_os_ag |

---

## 🏷️ Version
**Antigravity Experiment v2.0** - November 2026

---

*Built for institutional equity research excellence.*