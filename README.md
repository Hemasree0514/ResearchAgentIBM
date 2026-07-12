# Research Agent IBM

A lightweight AI research assistant powered by **IBM Granite-4** via watsonx.ai. Ask any research question and get detailed, well-reasoned, structured answers — directly in your browser.

---

## Features

- **Multi-turn conversations** — full message history is sent on every request so the model retains context across the entire session
- **Persistent chat history** — sessions are saved to `localStorage` and restored between page reloads
- **Multiple sessions** — start new chats, switch between past sessions, and delete individual sessions from the sidebar
- **Typing indicator** — animated dots while waiting for the model's response
- **IAM token caching** — the server fetches an IBM IAM bearer token once and reuses it until 60 seconds before expiry, minimising latency and API call

---

## Project Structure

```
research-agent/
├── public/
│   └── index.html       # Single-page frontend (chat UI)
├── server.js            # Express server + /api/chat endpoint
├── package.json
├── .env                 # Secret config (not committed)
└── README.md
```
## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | Vanilla HTML · CSS · JavaScript (no framework, no bundler) |
| Backend | Node.js · Express 4 |
| AI Model | IBM Granite-4 (`ibm/granite-4-h-small`) via watsonx.ai |
| Auth | IBM IAM (`grant_type: apikey` → Bearer token) |
| HTTP client | Axios |
| Config | dotenv |
---

## Getting Started

### 1. Clone the repository

```bash
git clone <your-repo-url>
cd research-agent
```

### 2. Install dependencies

```bash
npm install
```

### 3. Configure environment variables

Create a `.env` file in the project root:

```env
IBM_API_KEY=your_ibm_api_key
IBM_PROJECT_ID=your_watsonx_project_id
IBM_MODEL_ID=ibm/granite-4-h-small
IBM_CHAT_URL=https://eu-de.ml.cloud.ibm.com/ml/v1/text/chat?version=2024-05-01
IBM_IAM_URL=https://iam.cloud.ibm.com/identity/token
PORT=3000
```

> **Never commit `.env` to source control.** It is already listed in `.gitignore`.

### 4. Start the server

```bash
# Production
npm start

# Development (auto-restart on file changes)
npm run dev
```

Open your browser at [http://localhost:3000](http://localhost:3000).

```
## Model Parameters

| Parameter | Value |
|---|---|
| `max_new_tokens` | 2048 |
| `temperature` | 0.7 |

The system prompt instructs the model to act as a rigorous research assistant — providing comprehensive, well-structured answers, citing reasoning clearly, and acknowledging uncertainty where it exists.

---

## Chat Session Storage

Sessions are stored client-side in `localStorage` under the key `rAgent_sessions` as a JSON array:

```json
[
  {
    "id": "s_1720000000000",
    "title": "Explain quantum entanglement",
    "history": [
      { "role": "user",      "content": "..." },
      { "role": "assistant", "content": "..." }
    ],
    "createdAt": 1720000000000,
    "updatedAt": 1720000005000
  }
]
```

No data ever leaves the browser except the messages sent to your own server.


---

## License

MIT
