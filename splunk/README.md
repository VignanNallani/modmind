# ModMind (Splunk Integrated Version)

This directory contains the Splunk-integrated version of the ModMind AI-Powered Reddit Moderation Suite. It features a FastAPI backend that analyzes Reddit posts using Gemini AI and logs every moderation decision as an event to a Splunk HTTP Event Collector (HEC) endpoint.

## Directory Structure

```text
splunk/
├── backend/
│   ├── .env.example
│   ├── main.py
│   └── requirements.txt
├── frontend/
│   ├── index.html
│   ├── package.json
│   ├── vite.config.js
│   └── src/
└── README.md
```

## Backend Configuration

### Requirements
- Python 3.9+
- Packages listed in `requirements.txt`

### Environment Variables
Configure the following in your `.env` file under `splunk/backend/`:
- `GEMINI_API_KEY`: Your Google Gemini API Key.
- `SPLUNK_HEC_TOKEN`: Your Splunk HTTP Event Collector (HEC) token.
- `SPLUNK_HEC_URL`: The Splunk HEC event collector URL (default: `https://prd-p-z0n43.splunkcloud.com:8088/services/collector/event`).

### How It Works
Upon receiving a post analysis request (`POST /api/analyze`), the backend performs the following steps:
1. Calls the Gemini API to analyze the Reddit post content for sentiment, toxicity, and suggests a moderation action.
2. Formats a Splunk HEC event with the following payload structure:
   ```json
   {
     "event": {
       "post_id": "...",
       "subreddit": "...", 
       "title": "...",
       "toxicity_score": 0.0,
       "sentiment": "...",
       "action": "...",
       "timestamp": "..."
     },
     "sourcetype": "modmind_reddit",
     "index": "main"
   }
   ```
3. Sends the event to the Splunk HEC endpoint using HTTP POST with the `Authorization: Splunk <HEC_TOKEN>` header.

### Running the Backend Locally
1. Navigate to the backend directory:
   ```bash
   cd splunk/backend
   ```
2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
3. Create a `.env` file based on `.env.example` and populate the variables.
4. Run the FastAPI development server:
   ```bash
   python main.py
   ```
   The backend will be available at `http://localhost:8000`.

---

## Frontend Configuration

The frontend has been copied from the main frontend and updated to use relative paths for API requests. It is configured to run on port `3000` and proxy `/api` requests to the local backend on `http://localhost:8000` via Vite's proxy configurations.

### Running the Frontend Locally
1. Navigate to the frontend directory:
   ```bash
   cd splunk/frontend
   ```
2. Install dependencies:
   ```bash
   npm install
   ```
3. Start the Vite development server:
   ```bash
   npm run dev
   ```
   Open `http://localhost:3000` in your web browser.
