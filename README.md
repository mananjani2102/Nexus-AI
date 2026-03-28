# Nexus AI

**Nexus AI** is a full-stack web application for AI-assisted resume analysis. It accepts PDF or DOCX resumes, returns scoring and feedback (ATS-oriented metrics, strengths, weaknesses, keyword gaps, STAR-style bullet suggestions), and includes a bullet improver and a read-only analysis history view for the UI.

Repository: [github.com/mananjani2102/Nexus-AI](https://github.com/mananjani2102/Nexus-AI)

---

## Description

The product pairs a **React (Vite)** client with an **Express** REST API. The backend extracts text from uploads (PDF via `pdf-parse`; DOCX via a basic buffer read in the current implementation), invokes an LLM orchestration layer, and returns structured JSON. The frontend consumes these endpoints through a small Axios service layer and presents dashboards, upload flows, and suggestions.

---

## Features

- Resume upload (PDF and DOCX, server-side validation and size limit)
- Role-aware analysis with overall, ATS, and clarity scores
- Strengths, weaknesses, and ATS keyword gap lists
- STAR-method bullet mapping suggestions in the analysis payload
- Dedicated **Bullet Pro** endpoint to rewrite a single bullet with metrics
- **History** API returning past analyses (currently served from in-memory mock data)
- Responsive UI with Tailwind CSS v4, Framer Motion, and Lucide icons
- Health check endpoint for monitoring and smoke tests

---

## Tech Stack

| Area | Technologies |
|------|----------------|
| **Project type** | Full-stack (REST API + SPA) |
| **Frontend** | React 19, Vite 7, React Router 7, Tailwind CSS 4 (`@tailwindcss/vite`), Framer Motion, Lucide React, Axios |
| **Backend** | Node.js, Express 4, Multer, `pdf-parse`, `dotenv`, `cors` |
| **Database** | None (history is mock data in the current backend) |
| **Authentication** | None |
| **AI / LLM** | `backend/utils/llm.js` provides a **simulated** `callLLM` implementation; `GEMINI_API_KEY` is defined in `.env.example` for intended future Gemini integration |

---

## Installation and Setup

### Prerequisites

- **Node.js** 18 or newer (LTS recommended)
- **npm** (ships with Node)

### Clone the repository

```bash
git clone https://github.com/mananjani2102/Nexus-AI.git
cd Nexus-AI
```

### Backend

```bash
cd backend
npm install
cp .env.example .env
```

On Windows PowerShell: `Copy-Item .env.example .env`

Edit `.env` (see [Environment variables](#environment-variables)). Then:

```bash
npm run dev
```

The API listens on **`http://localhost:4000`** by default (`PORT` in `.env`).

### Frontend

```bash
cd frontend
npm install
npm run dev
```

The app runs at **`http://localhost:5173`** by default. The API client in `frontend/src/services/api.js` uses **`http://localhost:4000/api`** directly. Vite also defines a **`/api` proxy** to port `4000`, which you can use by pointing `baseURL` at a relative path (e.g. `/api`) if you prefer to avoid hard-coded origins in dev.

### Production build (frontend)

```bash
cd frontend
npm run build
npm run preview
```

For production, set the API base URL to your deployed backend (today the client uses `http://localhost:4000/api` in `frontend/src/services/api.js`).

---

## API documentation

**Base URL (local):** `http://localhost:4000/api`

| Method | Path | Description |
|--------|------|-------------|
| `GET` | `/api/health` | Service health and version metadata |
| `POST` | `/api/analyze` | Multipart upload: resume file + `jobRole` |
| `POST` | `/api/improve-bullet` | JSON body: `bullet`, optional `jobRole` |
| `GET` | `/api/history` | List of mock history entries (newest first) |

There are **no** `PUT` or `DELETE` routes in the current codebase.

### `GET /api/health`

**Response** `200 OK`

```json
{
  "status": "ok",
  "timestamp": "2026-03-28T12:00:00.000Z",
  "version": "1.0.0"
}
```

### `POST /api/analyze`

**Content-Type:** `multipart/form-data`

| Field | Type | Required | Notes |
|-------|------|----------|--------|
| `resume` | file | Yes | PDF or DOCX, max **5 MB** |
| `jobRole` | string | No | Defaults to `Software Engineer` |

**Success** `200 OK` — body shape returned by `callLLM` (example):

```json
{
  "overall_score": 72,
  "ats_score": 68,
  "clarity_score": 81,
  "strengths": ["..."],
  "weaknesses": ["..."],
  "ats_keywords_missing": ["CI/CD", "Kubernetes"],
  "star_bullets": {
    "Worked on the website front-end": "Led front-end development..."
  }
}
```

**Error examples**

```json
{ "error": "No file uploaded. Please attach a resume." }
```

```json
{ "error": "Could not extract readable text from the resume. Please ensure the file is not scanned or image-based." }
```

### `POST /api/improve-bullet`

**Content-Type:** `application/json`

```json
{
  "bullet": "Maintained the internal dashboard for the sales team.",
  "jobRole": "Software Engineer"
}
```

**Success** `200 OK`

```json
{
  "improved": "Architected and maintained...",
  "metrics": {
    "situation": "...",
    "action": "...",
    "result": "...",
    "impact_score": 8
  }
}
```

### `GET /api/history`

**Success** `200 OK` — JSON array of objects with fields such as `id`, `filename`, `job_role`, `date`, `overall_score`, `ats_score`, `clarity_score`, `score_history`, `keywords_missing`.

---

## Folder structure

```text
Nexus-AI/
├── README.md
├── backend/
│   ├── server.js
│   ├── package.json
│   ├── .env.example
│   ├── controllers/
│   │   ├── analyzeController.js
│   │   ├── bulletController.js
│   │   └── historyController.js
│   ├── routes/
│   │   ├── analyze.js
│   │   ├── bullet.js
│   │   └── history.js
│   └── utils/
│       └── llm.js
└── frontend/
    ├── index.html
    ├── package.json
    ├── vite.config.js
    ├── tailwind.config.js
    ├── postcss.config.js
    ├── public/
    ├── src/
    │   ├── main.jsx
    │   ├── App.jsx
    │   ├── App.css
    │   ├── index.css
    │   ├── components/
    │   │   ├── Navbar.jsx
    │   │   ├── PageWrapper.jsx
    │   │   ├── ErrorBanner.jsx
    │   │   ├── DemoVideoPlayer.jsx
    │   │   └── ScoreRing.jsx
    │   ├── pages/
    │   │   ├── LandingPage.jsx
    │   │   ├── UploadPage.jsx
    │   │   ├── DashboardPage.jsx
    │   │   ├── SuggestionsPage.jsx
    │   │   ├── BulletImproverPage.jsx
    │   │   └── HistoryPage.jsx
    │   ├── context/
    │   │   └── ResumeContext.jsx
    │   ├── services/
    │   │   └── api.js
    │   └── assets/
    │       ├── nexus-logo-01.png
    │       └── react.svg
    └── eslint.config.js
```

---

## Environment variables

Create **`backend/.env`** (copy from `backend/.env.example`).

| Variable | Required | Description |
|----------|----------|-------------|
| `PORT` | No | HTTP port for Express. Default: `4000` |
| `GEMINI_API_KEY` | No* | Placeholder in `.env.example` for future Google Gemini usage; **not read** by the current `llm.js` implementation |

\*The running app works without `GEMINI_API_KEY` because analysis uses the simulated `callLLM` helper until a real provider is wired in.

The frontend has **no** `.env` file in the repository; API base URL is set in code for local development.

---

## Error handling

- **Express JSON body** limit: `10mb` (see `server.js`).
- **Unknown routes:** `404` with `{ "error": "Endpoint not found" }`.
- **Unhandled errors:** Global middleware returns `err.status` or `500` with `{ "error": "<message>" }`.
- **Analyze route:** `400` for missing file; `422` for insufficient extracted text; `500` on analysis failure with message prefixed `Analysis failed:`.
- **Bullet route:** `400` for missing or too-short bullet; `500` on failure.
- **Multer:** Rejects disallowed MIME types with an error passed through the upload middleware.

---

## Deployment

This README documents **local** execution. For production:

1. **Backend:** Deploy the `backend` folder to any Node.js host (e.g. Render, Railway, Fly.io, AWS EC2/Elastic Beanstalk). Set `PORT` as required by the platform and configure CORS in `server.js` if the frontend origin changes.
2. **Frontend:** Build static assets (`npm run build`) and host on any static host (e.g. Netlify, Vercel, S3 + CloudFront). Update **`frontend/src/services/api.js`** `baseURL` to the public API origin, or introduce an environment-based configuration pattern.
3. **Secrets:** Never commit `.env`; configure secrets in the hosting provider’s dashboard.

A Netlify URL has been associated with this project in repository metadata; treat hosting choices as environment-specific.

---

## Future improvements

- Persist analysis history in a real database and scope it by user or session.
- Replace simulated `callLLM` with Google Gemini (or another provider) using `GEMINI_API_KEY`.
- Robust DOCX text extraction (e.g. dedicated library instead of raw buffer read).
- JWT or OAuth for authenticated uploads and history.
- Centralized API base URL via `import.meta.env` / build-time env for frontend.
- Automated tests (API integration and UI).

---

## Contributing

1. Fork the repository and create a feature branch from `main`.
2. Keep changes focused; follow existing code style in `frontend` and `backend`.
3. Do not commit secrets or large generated artifacts.
4. Open a pull request with a clear description of behavior changes and any new environment variables.

---

## License

Backend `package.json` declares **ISC**. Confirm `LICENSE` file in the repository if your organization requires an explicit license text; otherwise default to the terms stated in `package.json` and this README.

---

## Author

**[mananjani2102](https://github.com/mananjani2102)** — [Nexus-AI on GitHub](https://github.com/mananjani2102/Nexus-AI)

For questions or collaboration, use GitHub Issues or Pull Requests on the repository above.
