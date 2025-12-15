## 🌐 OSIVEX · Global News Intelligence & AI RAG System

OSIVEX (Open Source Intelligence Visualization & Exploration) is an AI-driven intelligence and analytics platform that collects, processes, summarizes, and analyzes open-source geopolitical data.
The system automatically scrapes international media sources (India, China, Russia), translates articles, summarizes content, classifies relevance, stores structured data, builds FAISS indexes, and performs advanced LangChain RAG retrieval through a modern Streamlit interface.

OSIVEX is designed as a fully functional AI analytics product, ideal for portfolio demonstration, interview scenarios, and real-world OSINT applications.

### 🧠 How OSIVEX Works
```
News → Clean Text → NLP Pipeline → FAISS Embeddings → LangChain RAG → Streamlit UI

1. Parsing Layer

✅ Crawl newest articles

✅ Extract text & metadata

✅ Translate via GPT

✅ Summarize content

✅ roduce normalized article objects

2. NLP Processing Layer

✅ YAKE keyword extraction

✅ RoBERTa relevance classification

3. Storage Layer

✅ Save data to JSON

✅ Optional PostgreSQL persistence

4. Vector Indexing (FAISS)

✅ Build multilingual & English summary indexes

5. Retrieval Layer (LangChain)

✅ Smart Mode: lightweight contextual retrieval

✅ Deep Mode: multi-hop reasoning with larger context

6. Final Output

✅ Clean, structured summary

✅ References with source URLs

✅ Interactive display in Streamlit dashboard

```
### 🚀 Live Demo
```

streamlit run osivex_app.py

```
### 🧩 Features
```
Data Ingestion

✅ Fetches latest geopolitical news (Russia, China, Brazil, India — extensible)
✅ Unified parser architecture for adding new countries/sources

AI Processing

✅ GPT translation and summarization
✅ YAKE keyword extraction
✅ RoBERTa relevance filtering
✅ Configurable Smart/Deep retrieval modes

Storage & Indexing

✅JSON + optional PostgreSQL support
✅FAISS vector store (multi-language + summaries)
✅ Automatic embedding refresh

Streamlit Dashboard

✅ Full-text semantic search
✅ Multi-language answers
✅ FAISS index selector
✅Debug console & RAG internals
✅ Source viewer with direct article links



```
### 🏗  Architecture Overview (End-to-End Data Flow)
```

User → run_all_parsers.py
        ↓
[Parsing Layer]
   ├─ Extract article URLs (Multi-source parser registry)
   ├─ Fetch full text & metadata
   └─ Normalize article objects
      ↓
[NLP Pipeline]
   ├─ Translation (GPT)
   ├─ Summarization (GPT)
   ├─ Keyword extraction (YAKE)
   └─ Relevance filtering (RoBERTa)
      ↓
[Storage Layer]
   ├─ Save to JSON
   └─ Insert into PostgreSQL (optional)
      ↓
[Vector Indexing]
   └─ Build FAISS index from multilingual embeddings
      ↓
[RAG Layer]
   ├─ Smart mode (light retrieval)
   └─ Deep mode (multi-hop)
      ↓
[UI Output – Streamlit]
   ├─ Final answer
   ├─ Source links
   └─ Search history



```
### 📂 Project Structure
```

osivex/
│
├─ data/                      # Parsed & processed JSON articles
├─ logs/                      # Parser execution logs
│
├─ parsers/                   # Country-specific news parsers
│   ├─ brazil/                   
│   ├─ china/                  
│   └─ russia/           
│
├─ chains/                    # LangChain RAG pipelines
│   ├─ pipeline_rag.py        #   Smart & Deep retrieval flows
│   ├─ retrieve_faiss.py      #   FAISS retriever logic
│   └─ smart_mode.py          #   Lightweight retrieval mode
│
├─ faiss_index/               # Local FAISS vector stores
│
├─ streamlit/                 # UI components
│   ├─ osivex_app.py          #   Main dashboard (v3)
│   └─ osivex_app_legacy.py   #   Previous version (reference)
│
├─ run_all_parsers.py         # Master script: RIA + TASS + Kommersant
├─ config.py                  # Global settings (toggles, env config)
├─ article_helpers.py         # Text cleaning, formatting, metadata utils
├─ requirements.txt           # Python dependencies
│
└─ README.md                  # Documentation (this file)


```
## 🗄️ Database (PostgreSQL) Integration
```
OSIVEX includes optional PostgreSQL storage for production-level data persistence.
Every parsed article — including original text, translated content, summaries,
keywords, metadata, and relevance scores — is stored in a normalized schema.

```
⏰ Automated Ingestion (Cron Jobs)
```
OSIVEX runs fully unattended on a remote Ubuntu server.

A daily cron schedule triggers run_all_parsers.py twice a day:

🕕 06:00 — morning collection

🕕 18:00 — evening collection

Each automated run:

✅ fetches fresh articles from all configured sources,

✅ processes them through the NLP pipeline (translation, summarization, relevance),

✅ inserts normalized records into PostgreSQL (articles table),

✅ writes cron output & errors into:
/home/ubuntu/osivex/logs/parsers_cron.log


```
### Stored Fields
```
| Column              | Description                                     |
|---------------------|-------------------------------------------------|
| source              | News provider (RIA, TASS, Kommersant, etc.)     |
| country             | Country of origin                               |
| title               | Original article title                          |
| url                 | Source link                                     |
| published_date      | Article publication date                        |
| original_content    | Extracted raw text                              |
| translated_content  | GPT-translated article                          |
| summary             | GPT summary                                     |
| keywords            | YAKE keyword extraction                         |
| relevance_score     | RoBERTa relevance classification                |
| collected_at        | Timestamp of OSIVEX ingestion                   |


```
### Environment Variables (.env)
```
DB_HOST=your_host
DB_PORT=5432
DB_NAME=osivex
DB_USER=postgres
DB_PASS=your_password

```
### Notes

```
- Database support is **optional** (JSON-only mode still works).
- Streamlit dashboard reads articles directly from PostgreSQL.
- Future analytics dashboards will run SQL-based queries.


```
### 🚀 Getting Started

```
1️⃣ Install dependencies:
pip install -r requirements.txt

2️⃣ Create a .env file and add your OpenAI key
(Add your OpenAI / HuggingFace keys used by the RAG pipeline)
OPENAI_API_KEY=your_key_here
HUGGINGFACEHUB_API_TOKEN=<your_hf_token>

3️⃣ Run parsers
(Collect news, translate/summarize, and save JSON dataset)
python run_all_parsers.py

4️⃣ Launch the RAG dashboard
(Streamlit UI for querying BRICS/SCO geopolitics)
streamlit run osivex_app.py

```
### ⚙️ Configuration (config.py)
```

| Toggle                            | Description                                 |
|-----------------------------------|---------------------------------------------|
| `DEBUG_MODE`, `MAX_ARTICLES`      | Limit article count for quick testing       |
| `USE_GPT`                         | Enable GPT translation & summarization      |
| `USE_FAISS`                       | Toggle FAISS vector index for RAG           |
| `SMART_MODE`, `DEEP_MODE`         | Control retrieval depth (Light vs Deep)     |
| `LANGUAGE`, `TEMPERATURE`         | Tune GPT response settings                  |
| `ROBERTA_FILTER`                  | Enable XLM-RoBERTa relevance classification |
| `ROBERTA_VERBOSE_LOGS`            | Show detailed relevance-filter logs         |

```

### 📊 Example Logs

```text
2025-08-14 15:02:07 — INFO — ✔ Article saved
2025-08-14 15:02:08 — INFO — Filtered and saved 1 out of 10 articles for keyword "SCO"
2025-08-14 15:02:08 — INFO — Saved to data/kommersant_sco_today_en.json
2025-08-14 15:02:08 — INFO — >>> Kommersant parser completed.

=== Per-parser durations ===
RIA 5.45 s OK
TASS 3.41 s OK
Kommersant 22.38 s OK

=== All parsers finished at 2025-08-14 15:02:08 ===
Total duration: 00:00:31


```
### 🗂 Example JSON Output


```json

{
  "source": "TASS",
  "keyword": "BRICS",
  "collected_at": "2025-11-04T15:02:08Z",
  "vectorized": true,
  "articles": [
    {
      "title": "BRICS leaders meet in Kazan",
      "url": "https://tass.ru/...",
      "date": "2025-11-04",
      "summary": "Discussion on expanding trade within BRICS.",
      "embedding_id": "vec_00123",
      "relevance": 0.92,
      "keywords": ["BRICS", "trade", "Kazan"]
    }
  ]
}
```

### 📅 Roadmap (Aug–Dec 2025)
```
Phase	   Goal
Phase 1	   Refactor RAG pipeline (LangChain LCEL)
Phase 2	   Add analytics dashboard (Streamlit visuals)
Phase 3	   Automate FAISS embedding updates
Phase 4	   Deploy FastAPI endpoint for external RAG queries

```
### 🧠 Technologies Used
```

• Python 3.11+
• BeautifulSoup + Requests (web-scraping)
• LangChain (RAG orchestration)
• FAISS (vector indexing)
• OpenAI GPT API (translation & summarization)
• RoBERTa (relevance classification)
• YAKE (keyword extraction)
• Streamlit (front-end dashboard)
• PostgreSQL + JSON (storage and persistence)

```
### 📄 License (Private Use Only)
```
This repository is part of the OSIVEX AI Research Suite and is provided solely for
private research, portfolio demonstration, and educational purposes.

© 2025 Artiom Kisliakov  
Project: OSIVEX v6  
Modules included: AI RAG, NLP Processing, Streamlit Visualization  
Edition: Internal Research Release (2025–2026)

All rights reserved.  
Commercial use, redistribution, copying, or integration into external products is strictly prohibited
without prior written permission from the author.
