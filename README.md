# ⚡ Nexus AI — Resume Optimizer

An AI-powered resume analysis platform that provides enterprise-grade intelligence — surfacing keyword gaps, ATS score, clarity metrics, and STAR-method bullet improvements in seconds.

![Nexus AI](https://img.shields.io/badge/Nexus-AI-06b6d4?style=for-the-badge&logo=data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjQiIGhlaWdodD0iMjQiIHZpZXdCb3g9IjAgMCAyNCAyNCIgZmlsbD0ibm9uZSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj48cGF0aCBkPSJNMTMgMkwzIDE0aDlsLTEgMTAgMTAtMTJoLTlsMi0xMHoiIGZpbGw9IiMwNmI2ZDQiLz48L3N2Zz4=)
![React](https://img.shields.io/badge/React-19-61DAFB?style=flat-square&logo=react)
![Node.js](https://img.shields.io/badge/Node.js-Express-339933?style=flat-square&logo=node.js)
![TailwindCSS](https://img.shields.io/badge/Tailwind-CSS-06B6D4?style=flat-square&logo=tailwindcss)

## 🚀 Features

- **ATS Score Analysis** — Evaluate how well your resume performs against Applicant Tracking Systems
- **Keyword Gap Detection** — Identify missing keywords for your target role
- **Clarity Score** — Get feedback on readability and impact of your content
- **STAR Bullet Improver** — AI-powered bullet point rewriting using the STAR method
- **Analysis History** — Track your resume score improvements over time
- **Beautiful Dashboard** — Interactive visualizations with sparkline charts

## 🛠️ Tech Stack

| Layer      | Technology                              |
| ---------- | --------------------------------------- |
| Frontend   | React 19, Vite, Tailwind CSS, Framer Motion |
| Backend    | Node.js, Express.js                     |
| AI Engine  | Google Gemini API                       |
| Icons      | Lucide React                            |

## 📦 Project Structure

```
Nexus-AI/
├── frontend/               # React + Vite frontend
│   ├── src/
│   │   ├── components/     # Reusable UI components
│   │   ├── pages/          # Route-level page components
│   │   ├── context/        # React context providers
│   │   └── services/       # API service layer
│   └── public/             # Static assets
├── backend/                # Express.js API server
│   ├── controllers/        # Route handler logic
│   ├── routes/             # Express route definitions
│   └── utils/              # Utility functions (LLM integration)
└── README.md
```

## ⚙️ Getting Started

### Prerequisites

- Node.js v18+
- npm or yarn
- Google Gemini API Key

### 1. Clone the Repository

```bash
git clone https://github.com/mananjani2102/Nexus-AI.git
cd Nexus-AI
```

### 2. Backend Setup

```bash
cd backend
npm install
cp .env.example .env
# Add your Gemini API key to .env
npm run dev
```

The backend will start at `http://localhost:4000`

### 3. Frontend Setup

```bash
cd frontend
npm install
npm run dev
```

The frontend will start at `http://localhost:5173`

## 🔐 Environment Variables

Create a `.env` file in the `backend/` directory:

```env
PORT=4000
GEMINI_API_KEY=your_gemini_api_key_here
```

## 📄 License

This project is licensed under the ISC License.
