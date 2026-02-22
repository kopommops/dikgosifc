# Dikgosi FC

> The first dedicated data & intelligence platform for the **FNB Botswana Premier League**.

[![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://codespaces.new/kopommops/dikgosi-fc)

---

## What is Dikgosi FC?

**Dikgosi** (Setswana: *Kings / Chiefs*) is a full-stack sports data platform built exclusively for the Botswana Premier League. It scrapes live data, transforms it with dbt, runs ML predictions, and serves everything through a FastAPI backend and a clean, responsive frontend.

### Features (MVP)
| Feature | Status |
|---------|--------|
| League Standings | 🟡 In Progress |
| Upcoming Fixtures | 🟡 In Progress |
| Match Predictions (ML) | 🔴 Planned |
| Fantasy League | 🔴 Planned |
| News Feed (Africa Soccer Zone) | 🔴 Planned |

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Scraping | Python + Playwright + BeautifulSoup4 |
| Orchestration | Prefect |
| Database | PostgreSQL (bronze / silver / gold schemas) |
| Transformation | dbt Core + dbt-postgres |
| ML | LightGBM + scikit-learn |
| API | FastAPI + JWT Auth |
| Frontend | Vanilla JS + Chart.js |
| Dev Environment | GitHub Codespaces + Docker Compose |

---

## Quick Start (GitHub Codespaces)

1. Click **"Open in GitHub Codespaces"** above
2. Wait ~2 minutes for the environment to build
3. The post-create script will auto-install everything

Then in the terminal:

```bash
# Start the API
cd api && uvicorn main:app --reload --host 0.0.0.0 --port 8000

# Run the scraper (dry run first)
python scraper/flashscore.py --mode results --dry-run

# Run dbt models
cd dbt_bpl && dbt run

# Serve the frontend
cd frontend && python -m http.server 4200
```

---

## Local Setup (without Codespaces)

```bash
git clone https://github.com/YOUR_USERNAME/dikgosi-fc.git
cd dikgosi-fc

# Copy env vars
cp .env.example .env

# Start PostgreSQL
docker-compose -f .devcontainer/docker-compose.yml up -d db

# Install Python deps
pip install -r requirements.txt

# Init DB
python scripts/init_db.py

# Install Playwright browsers
playwright install chromium

# Install dbt packages
cd dbt_bpl && dbt deps
```

---

## Project Structure

```
dikgosi-fc/
├── .devcontainer/          # Codespaces config
├── scraper/                # Playwright scrapers
│   ├── flashscore.py       # Results & fixtures
│   └── sofascore.py        # Player stats (Week 3)
├── dbt_bpl/                # dbt transformation project
│   └── models/
│       ├── staging/        # stg_matches, stg_teams
│       ├── intermediate/   # int_team_form, int_h2h
│       └── marts/          # mart_standings, mart_predictions
├── ml/                     # ML training & serving
├── api/                    # FastAPI application
│   └── routers/            # standings, fixtures, predict, news
├── frontend/               # Vanilla JS SPA
└── flows/                  # Prefect orchestration flows
```

---

## API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/health` | Health check |
| POST | `/api/auth/login` | Get JWT token |
| GET | `/api/standings/?season=2025-26` | League table |
| GET | `/api/fixtures/upcoming` | Next matches |
| GET | `/api/fixtures/results` | Recent results |
| POST | `/api/predict/match` | ML prediction |
| GET | `/api/news/` | News feed |

Full docs: `http://localhost:8000/docs`

---

## dbt Models

```
stg_matches         → cleaned match results
stg_teams           → normalised team reference
int_team_form       → last-5 form per team
int_head_to_head    → H2H record between any two teams (Week 3)
mart_league_standings → final league table (gold)
mart_prediction_features → ML features (Week 6)
```

---

*Built by Mmopiemang Mmopiemang · Portfolio Project · 2026*
