# ModMind 🛡️
AI-Powered Reddit Moderation Suite

## Features
- Fetch posts from any subreddit
- AI analysis using Gemini (sentiment, toxicity, mod actions)
- Subreddit statistics dashboard
- Clean dark theme UI

## Tech Stack
- Frontend: React + Tailwind CSS + Vite
- Backend: Python FastAPI
- AI: Google Gemini API
- Data: Reddit Public API

## Setup
### Backend
cd backend
pip install -r requirements.txt
Add GEMINI_API_KEY to .env
uvicorn backend.main:app --reload

### Frontend
cd frontend
npm install
npm run dev

## License
MIT
