# ModMind — Splunk Integration Architecture

## Data Flow

```mermaid
graph TD
    classDef browser fill:#57a1f8,stroke:#2b6bb8,stroke-width:2px,color:#fff;
    classDef frontend fill:#7c3aed,stroke:#5b21b6,stroke-width:2px,color:#fff;
    classDef backend fill:#10b981,stroke:#047857,stroke-width:2px,color:#fff;
    classDef splunk fill:#f97316,stroke:#c2410c,stroke-width:2px,color:#fff;
    classDef external fill:#6b7280,stroke:#374151,stroke-width:2px,color:#fff;

    Browser[User Browser]:::browser
    Frontend[React Frontend<br/>Vercel]:::frontend
    Backend[FastAPI Backend<br/>Render]:::backend
    Reddit[Reddit API / Mock Fallback]:::external
    Gemini[Google Gemini AI<br/>gemini-flash-latest]:::external
    Splunk[Splunk HEC<br/>HTTP Event Collector]:::splunk
    Dashboard[Splunk Cloud Dashboard<br/>ModMind Moderation Dashboard]:::splunk

    Browser --> Frontend
    Frontend --> Backend
    Backend -->|1. Fetch posts| Reddit
    Backend -->|2. Analyze content| Gemini
    Backend -->|3. Log decisions| Splunk
    Splunk --> Dashboard
```

```text
User Browser
↓
React Frontend (Vercel)
↓
FastAPI Backend (Render)
↓ (1) Fetch posts
Reddit API / Mock Fallback
↓ (2) Analyze content
Google Gemini AI (gemini-flash-latest)
↓ (3) Log decisions
Splunk HEC (HTTP Event Collector)
↓
Splunk Cloud Dashboard
(ModMind Moderation Dashboard)
```

## Components

### Frontend
- React + Tailwind CSS
- Deployed on Vercel
- URL: https://modmind-splunk.vercel.app

### Backend
- FastAPI + Python
- Deployed on Render
- URL: https://modmind-splunk-backend.onrender.com

### AI Engine
- Google Gemini via google-genai SDK
- Model: gemini-flash-latest
- Returns: sentiment, toxicity score, action, confidence

### Splunk Integration
- HTTP Event Collector (HEC)
- Sourcetype: modmind_reddit
- Index: main
- Events contain: post_id, subreddit, title, 
  toxicity_score, sentiment, action, timestamp

### Splunk Dashboard
- Total Moderation Decisions (Single Value)
- Moderation Actions Breakdown (Bar Chart)
- Sentiment Distribution (Pie Chart)
- Toxicity Score Over Time (Line Chart)

## Track
Security — AI-powered threat detection and 
content moderation for Reddit communities
