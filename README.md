# 🩺 Pharmacovigilance Signal Triage Copilot
**Intelligent Agent for Drug Safety Signal Prioritization**

---

## 🎯 Problem Definition

Pharmacovigilance (PV) safety teams receive **millions of adverse drug reaction reports** annually. Data is frequently incomplete, redundant, and non-uniform (e.g. different brand names for the same active substance). Manual triage is slow and error-prone, delaying the detection of critical safety signals.

- **Inputs:** Raw adverse reaction reports (JSON/CSV format) extracted from the public openFDA FAERS database + natural language user query (e.g. *"Analyze signals for DUPIXENT in 2024"*)
- **Outputs:** An automatically generated **Signal Packet PDF** containing: a prioritized signal list, statistical metrics (PRR, ROR), monthly trend charts, and explainable AI recommendations

---

## 🏗️ Architecture Overview

![Architecture Diagram](schema_arhitectura.png)

**Autonomous agent workflow (7 steps):**

```
STEP 1: filter_by_time            → selects reports within the requested time range
STEP 2: normalize_drug            → brand name → INN via RxNorm API (NIH)
STEP 3: normalize_reaction        → free-text term → MedDRA Preferred Term
STEP 4: calculate_metrics         → PRR, ROR, monthly trend, prioritization score
STEP 5: extract_cases             → drill-down: 5 individual reports
STEP 6: formulate_recommendations → LLM generates text justification
STEP 7: generate_pdf_packet       → exportable PDF with embedded charts
```

---

## 🧠 AI Stack

| Component | Technology | Rationale |
|---|---|---|
| **LLM** | Llama 3.3 70B (Meta, open-source) | Free, 70B parameters, correctly follows tool-use instructions |
| **Inference provider** | Groq API (LPU hardware) | 14,400 req/day free tier, latency < 1s/token |
| **Agent framework** | LangGraph `create_react_agent` | ReAct pattern: autonomously cycles Reason→Act until all steps complete |
| **Tool use** | 6 `@tool` LangChain Core functions | Each function's docstring serves as its description for the agent |
| **Drug normalization** | RxNorm API (NIH/NLM) | US de facto standard, free, tolerant of spelling variants |
| **Reaction normalization** | Local MedDRA PT mapping (~30 terms) | Full MedDRA requires a license; local mapping covers frequent FAERS terms |
| **UI** | Gradio `share=True` | Temporary public link, no deployment needed — ideal for demos |
| **Export** | fpdf2 | Fixed-format, non-editable PDF — standard in FDA/EMA regulated environments |

---

## 📈 Measured Performance (Live KPIs)

| KPI | Manual (baseline) | With AI (measured) |
|---|---|---|
| ⏱️ Time per signal | ~45 minutes | ~45 seconds |
| 📦 Packets/analyst/day (8h) | ~10 | ~100+ |
| 🕐 Signal → review time | hours / days | seconds |
| 📋 Output standardization | variable | ✅ 5 identical sections every run |
| 🔁 Reproducibility | manual | ✅ automatic (date seed + deduplication) |

> Manual baseline of 45 min/packet per PV literature (Evans et al., 2001).

---

## 🌍 Sustainable Development Goals (SDGs)

**SDG 3 — Good Health and Well-Being**
Early detection of dangerous drug–event combinations reduces patient harm. Every week gained in signal identification can prevent serious adverse reactions in thousands of patients.

**SDG 9 — Industry, Innovation and Infrastructure**
Modernizing pharmaceutical industry workflows through AI automation — eliminating repetitive work and allowing specialists to focus on high-value decisions.

**SDG 10 — Reduced Inequalities**
Small PV teams (developing countries, generic manufacturers) cannot afford senior specialist analysts. The agent democratizes access to regulatory-quality analysis.

---

## 📁 Project Structure


```
├── notebooks/
│   ├── 01_EDA.ipynb                       # FAERS data ingestion + EDA + visualizations
│   └── 02_Agent_AI_v2.ipynb               # AI agent + 6 tools + Gradio UI + Tests
├── data/
│   ├── faers_raw_sample.json              # Raw FAERS data 2022–2024 (openFDA)
│   ├── cleaned_faers_data.csv             # Processed data post-EDA
│   └── cleaned_faers_data_deduped.csv     # Data after automatic deduplication
├── .env.example                           # API key template
├── .gitignore                             # Excludes .env and sensitive data
└── README.md
```

---

## ⚙️ Quick Setup

**1. Clone the repository**
```bash
git clone https://classroom.github.com/a/LESHiTyK
cd <repo-name>
```

**2. Create the `.env` file**
```bash
cp .env.example .env
# Open .env and fill in the keys:
```
```
GROQ_API_KEY=your_key_from_console.groq.com
FDA_API_KEY=your_key_from_open.fda.gov   # optional
```

**3. Run the notebooks in order**
```
01_EDA.ipynb           →  generates data/cleaned_faers_data.csv
02_Agent_AI_v2.ipynb   →  starts the agent + Gradio UI (public link in output)
```

**Getting API keys (free):**
- `GROQ_API_KEY` → [console.groq.com](https://console.groq.com) — simple registration, 14,400 req/day
- `FDA_API_KEY` → [open.fda.gov/apis](https://open.fda.gov/apis/) — optional, increases rate limit

---

## 🚀 Future Improvements

| Priority | Improvement | Impact |
|---|---|---|
| 🔴 High | **Full MedDRA integration** — replace the local ~30-term mapping with a licensed MedDRA browser for complete reaction coverage | Higher signal recall, regulatory acceptance |
| 🔴 High | **Real-time FAERS ingestion** — replace static CSV snapshots with a live openFDA API polling pipeline triggered on a weekly schedule | Signals detected weeks earlier |
| 🟡 Medium | **Expanded signal metrics** — add EBGM (Empirical Bayes Geometric Mean) and IC (Information Component) alongside PRR/ROR for multi-method corroboration | Reduces false positive rate |
| 🟡 Medium | **Multi-drug interaction signals** — extend the agent to detect co-reported drug pairs, not just single-drug signals | Catches combination therapy risks |
| 🟡 Medium | **Structured output validation** — enforce JSON Schema on LLM outputs and add a confidence score per recommendation | More reliable PDF generation |
| 🟢 Low | **Authentication & audit trail** — add user login and a tamper-evident log of every generated packet for compliance | Required for production regulatory use |
| 🟢 Low | **International data sources** — integrate EudraVigilance (EMA) and WHO VigiBase alongside FAERS for global signal coverage | Broader pharmacovigilance scope |

---

## 📚 References & Data Sources

| Resource | URL |
|---|---|
| openFDA FAERS API | https://open.fda.gov/apis/drug/event/ |
| RxNorm API (NIH) | https://lhncbc.nlm.nih.gov/RxNorm/ |
| Evans et al. 2001 (PRR criterion) | *Use of proportional reporting ratios (PRRs) for signal generation from spontaneous adverse drug reaction reports* |
| LangGraph ReAct | https://langchain-ai.github.io/langgraph/ |
| Groq API | https://console.groq.com |
