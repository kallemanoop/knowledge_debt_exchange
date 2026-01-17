```

┌─────────────────────────────────────────────────────────────┐
│                        FRONTEND                             │
│  (React/Next.js or Streamlit for demo)                     │
│  - Profile Management                                       │
│  - Match Display                                            │
│  - Barter Cycle Visualization                              │
└──────────────────────┬──────────────────────────────────────┘
                       │ REST API
                       ▼
┌─────────────────────────────────────────────────────────────┐
│                    BACKEND (FastAPI)                        │
│                                                             │
│  ┌───────────────────────────────────────────────────┐    │
│  │           MATCHING SERVICE (HYBRID)               │    │
│  │                                                   │    │
│  │  Step 1: Candidate Retrieval                     │    │
│  │  ┌─────────────────────────────────────┐        │    │
│  │  │   EMBEDDING SERVICE                 │        │    │
│  │  │   - Embed user need                 │        │    │
│  │  │   - Embed all skills                │        │    │
│  │  │   - Cosine similarity               │        │    │
│  │  │   - Return top-K (K=10-20)          │        │    │
│  │  └─────────────────────────────────────┘        │    │
│  │                    ▼                              │    │
│  │  Step 2: Intelligent Re-ranking                  │    │
│  │  ┌─────────────────────────────────────┐        │    │
│  │  │   LLM SERVICE (Agentic)             │        │    │
│  │  │   - Analyze context                 │        │    │
│  │  │   - Check prerequisites             │        │    │
│  │  │   - Assess skill level match        │        │    │
│  │  │   - Generate explanation            │        │    │
│  │  │   - Output structured JSON          │        │    │
│  │  └─────────────────────────────────────┘        │    │
│  │                    ▼                              │    │
│  │  Step 3: Reciprocity & Barter Detection         │    │
│  │  ┌─────────────────────────────────────┐        │    │
│  │  │   BARTER SERVICE                    │        │    │
│  │  │   - Check reverse matches           │        │    │
│  │  │   - Detect 3-way cycles             │        │    │
│  │  │   - Calculate fairness score        │        │    │
│  │  └─────────────────────────────────────┘        │    │
│  └───────────────────────────────────────────────────┘    │
│                                                             │
│  ┌───────────────────────────────────────────────────┐    │
│  │         STORAGE LAYER                             │    │
│  │  - SQLite (demo) / Supabase (production)         │    │
│  │  - User profiles, skills, needs                   │    │
│  │  - Embedding cache                                │    │
│  │  - Match history                                  │    │
│  └───────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

```
# file structure
knowledge-debt-exchange/
├── README.md
├── requirements.txt
├── .env.example
├── .gitignore
├── docker-compose.yml (optional)
│
├── data/
│   ├── seed_users.json
│   └── app.sqlite3 (runtime)
│
├── frontend/ (NEW - React/Next.js)
│   ├── package.json
│   ├── src/
│   │   ├── components/
│   │   │   ├── ProfileForm.jsx
│   │   │   ├── MatchCard.jsx
│   │   │   ├── BarterCycle.jsx
│   │   │   └── Dashboard.jsx
│   │   ├── pages/
│   │   │   ├── index.jsx
│   │   │   ├── profile.jsx
│   │   │   └── matches.jsx
│   │   ├── services/
│   │   │   └── api.js
│   │   └── utils/
│   └── public/
│
├── backend/ (RESTRUCTURED)
│   ├── main.py (FastAPI entry point)
│   ├── requirements.txt
│   │
│   ├── api/
│   │   ├── __init__.py
│   │   ├── routes/
│   │   │   ├── users.py
│   │   │   ├── matching.py
│   │   │   └── barter.py
│   │   └── middleware/
│   │       └── auth.py (future)
│   │
│   ├── core/
│   │   ├── __init__.py
│   │   ├── config.py
│   │   ├── database.py (SQLite/Supabase adapter)
│   │   └── types.py
│   │
│   ├── services/
│   │   ├── __init__.py
│   │   ├── embedding_service.py (NEW - upgraded)
│   │   ├── llm_service.py (NEW - agentic matching)
│   │   ├── matching_service.py (HYBRID)
│   │   ├── barter_service.py
│   │   └── storage_service.py
│   │
│   ├── models/
│   │   ├── __init__.py
│   │   ├── user.py
│   │   ├── skill.py
│   │   ├── match.py
│   │   └── barter.py
│   │
│   └── utils/
│       ├── __init__.py
│       ├── similarity.py
│       └── validation.py
│
├── streamlit_legacy/ (KEEP for quick demos)
│   └── app.py (current Streamlit app)
│
└── tests/
    ├── test_matching.py
    ├── test_embeddings.py
    └── test_llm_service.py
```