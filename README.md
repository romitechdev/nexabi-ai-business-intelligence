# NexaBI — Next-Generation Business Intelligence Platform

An AI-powered business analytics platform for customer analytics and product recommendation in the retail industry. Built on top of the Global Superstore dataset (51,290 transactions, 2012–2015).

**Capstone Project — PJK-GM040 | Pijak × IBM SkillsBuild**
Theme: *AI for Business Intelligence and Market Insights*

---

## Key Features

| Page | Description |
|---|---|
| **Overview** | Summary KPIs, Loyal/Passive cluster distribution, AI Smart Advisor |
| **Sales Performance** | Revenue per segment, monetary distribution, recency, Pareto contribution, AI Sales Forecast |
| **Analytics** | RFM scatter plot, segment distribution, loyalty %, detailed RFM table |
| **Market Basket** | Apriori association rules (106 rules), top bundling ideas |
| **Top Customers** | Top-3 podium + ranked table of the 10 highest customers |
| **Churn Risk** | Monitor at-risk customers + AI retention strategies |
| **Customers** | Full CRUD, search, pagination, CSV export |
| **AI Chatbot** | Contextual floating chatbot on all dashboard pages |

---

## Project Structure

```text
.
├── backend/
│   ├── app/
│   │   ├── ai_helper.py       # OpenAI-compatible AI client
│   │   ├── ai_routes.py       # AI endpoints: insight, chat, forecast
│   │   ├── analytics_routes.py # Extended analytics endpoints
│   │   ├── auth.py
│   │   ├── config.py
│   │   ├── database.py
│   │   ├── main.py
│   │   ├── models.py
│   │   ├── schemas.py
│   │   └── seeder.py
│   ├── df_kmeans.csv              # K-Means RFM data (1,590 customers)
│   ├── association_rules.csv      # 106 Apriori association rules
│   ├── Market Basket Analysis (Apriori).ipynb
│   ├── docker-compose.yml
│   ├── Dockerfile
│   ├── requirements.txt
│   └── .env
└── frontend/
    ├── src/
    │   ├── pages/             # 9 dashboard pages
    │   ├── components/        # Sidebar, ChatbotWidget, etc.
    │   ├── layouts/
    │   └── api/axios.js       # Axios + JWT interceptor
    ├── package.json
    └── vite.config.js
```

---

## Prerequisites

- Docker & Docker Compose (recommended)
- Or: Python 3.10+, Node.js 20+, PostgreSQL 15

---

## Run with Docker (Recommended)

### 1. Create the Docker network

```bash
docker network create nexabi_shared_net
```

### 2. Start the backend (FastAPI + PostgreSQL)

```bash
cd backend
docker compose up --build -d
```

Backend available at `http://localhost:5000`
Swagger docs: `http://localhost:5000/docs`

### 3. Start the frontend (React + Nginx)

```bash
cd frontend
docker compose up --build -d
```

Frontend available at `http://localhost:3000`

---

## Run Without Docker

### Backend

```bash
cd backend
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
cp .env.example .env   # adjust DATABASE_URL and SECRET_KEY
uvicorn app.main:app --host 0.0.0.0 --port 5000 --reload
```

### Frontend

```bash
cd frontend
npm install
# Create a .env pointing to the local backend:
echo "VITE_API_BASE_URL=http://localhost:5000/api" > .env
npm run dev
```

---

## Environment Variables (backend/.env)

| Variable | Description |
|---|---|
| `DATABASE_URL` | PostgreSQL connection |
| `SECRET_KEY` | Secret used for signing JWTs |
| `CSV_FILE_PATH` | Path to the RFM data CSV file (default: `df_kmeans.csv`) |
| `CORS_ORIGINS` | Allowed frontend origins (comma-separated) |
| `OPENAI_BASE_URL` | Base URL of the OpenAI-compatible API for AI features |
| `OPENAI_API_KEY` | API key for the AI service |
| `OPENAI_MODEL` | Name of the model used |
| `GEMINI_API_KEY` | (Optional) Gemini API key |

---

## Complete API Endpoints

### Authentication
| Method | Endpoint | Description |
|---|---|---|
| POST | `/api/auth/register` | Register a new user |
| POST | `/api/auth/login` | Login, returns a JWT token |

### Analytics
| Method | Endpoint | Description |
|---|---|---|
| GET | `/api/analytics/overview` | Summary KPIs (total, loyal, passive, avg monetary) |
| GET | `/api/analytics/rfm-scatter` | RFM scatter plot data for all customers |
| GET | `/api/analytics/segment-stats` | Statistics per segment |
| GET | `/api/analytics/top-customers` | Top 10 customers by monetary |
| GET | `/api/analytics/churn-risk` | List of at-risk customers |
| GET | `/api/analytics/market-basket` | Apriori association rules |
| GET | `/api/analytics/sales-performance` | Revenue, orders, distribution per segment |

### AI Features
| Method | Endpoint | Description |
|---|---|---|
| GET | `/api/analytics/ai-insight` | Generate an AI insight from the overview data |
| GET | `/api/analytics/churn-ai` | Churn analysis + AI retention strategies |
| GET | `/api/analytics/sales-forecast` | Next month's sales forecast |
| POST | `/api/analytics/chat` | Contextual interactive chatbot |

### Customer CRUD
| Method | Endpoint | Description |
|---|---|---|
| GET | `/api/customers` | List all customers |
| POST | `/api/customers` | Add a new customer |
| PUT | `/api/customers/{customer_id}` | Update customer data |
| DELETE | `/api/customers/{customer_id}` | Delete a customer |

> All endpoints except auth require the header: `Authorization: Bearer <token>`

---

## Auto-Start with Systemd (VPS/VM)

```bash
sudo cp backend/nexabi-backend.service /etc/systemd/system/
sudo systemctl daemon-reload
sudo systemctl enable --now nexabi-backend
```

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React 19, Vite, Tailwind CSS, Recharts, Lucide |
| Backend | FastAPI, SQLAlchemy, Pydantic, Uvicorn |
| Database | PostgreSQL 15 |
| ML | Scikit-learn (K-Means), MLxtend (Apriori) |
| AI | OpenAI-compatible API (Qwen/LLM) |
| DevOps | Docker, Docker Compose, Nginx, Systemd |

---

## Dataset

**Global Superstore Sales Dataset** — Kaggle (CC0: Public Domain)
51,290 rows, 24 columns, 2012–2015 period
[kaggle.com/datasets/apoorvaappz/global-super-store-dataset](https://kaggle.com/datasets/apoorvaappz/global-super-store-dataset)

---

## Team

**Team ID:** PJK-GM040 | Pijak × IBM SkillsBuild

| Name | Learning Path | Responsibilities |
|---|---|---|
| Michael Sanjaya | Machine Learning | Data collection, cleaning, EDA, preprocessing pipeline |
| Irisaliya Irhabiyah Banat | Machine Learning | Feature engineering, K-Means clustering, Apriori MBA, model evaluation |
| Muhromin | Backend | REST API, database, authentication |
| Ahmad Fauzul Adhim | Frontend | Dashboard UI/UX, interactive charts, responsiveness |
| Muhammad Daffa Amrullah | DevOps | CI/CD, Docker, cloud deployment, technical documentation |
