---
inclusion: always
---

# Technology Stack

## Backend

- **Python 3.x** - Server runtime
- **HTTP Server** (SimpleHTTPRequestHandler) - File serving and REST endpoints
- **WebSockets** (websockets library) - Real-time chat communication
- **Google Gemini API** - AI responses (gemini-2.5-flash-lite model)
- **pypdf** - PDF text extraction

## Frontend

- **Vanilla JavaScript** - No frameworks
- **HTML5/CSS3** - UI structure and styling
- **WebSocket API** - Client-side real-time communication
- **localStorage** - Client-side data persistence (main app only)
- **Shadow DOM** - SDK isolation (main app only)

## Architecture Patterns

- **Iframe isolation** - SDK, chat, and sidebar run in separate iframes (main app)
- **WebSocket communication** - Eliminates page reloads during chat
- **HTTP polling** - Shared thoughts poll server every 2 seconds
- **Event-driven** - User interactions trigger async operations
- **Multi-client sync** - All clients share same server data

## Common Commands

### Start the server
```bash
python combined_server.py
```

This starts both:
- HTTP server on port 5000 (file serving, uploads, thoughts API)
- WebSocket server on port 8765 (chat communication)

### Install dependencies
```bash
pip install -r requirements.txt
```

Required packages:
- `google-genai` - Google Gemini AI API
- `websockets` - WebSocket server
- `pypdf` - PDF text extraction

### Access the applications
- Main app: `http://localhost:5000` or `http://localhost:5000/iframeindex.html`
- Client app: `http://localhost:5000/clientapp/client.html`

## Server Endpoints

### HTTP Endpoints (Port 5000)
- `GET /` - Serves iframeindex.html
- `GET /get_thoughts` - Returns shared thoughts JSON
- `POST /add_thought` - Adds a new shared thought
- `POST /upload_context` - Uploads files and extracts PDF text
- `GET /*` - Static file serving

### WebSocket Endpoint (Port 8765)
- Receives: `{ "message": "user message" }`
- Sends: `{ "success": true, "user_message": "...", "ai_response": "..." }`
- Includes file context and shared thoughts in AI prompts

## API Keys

- Google Gemini API key is hardcoded in `combined_server.py`
- LintasEdu SDK credentials are in `hackathon.html`

## File Storage

### Server-side (Shared across all clients)
- `messages.json` - Chat history
- `shared_thoughts.json` - Shared page thoughts
- `file_context.json` - Uploaded document metadata
- `uploaded_files/` - Original PDFs and extracted .txt files

### Client-side (Main app only)
- `localStorage.chatHistory` - Local chat cache
- `localStorage.uploadedFiles` - Local file list cache

## PDF Processing

When a PDF is uploaded:
1. Client sends file as base64 via POST to `/upload_context`
2. Server decodes and saves original PDF to `uploaded_files/`
3. Server extracts text using pypdf
4. Server saves extracted text as `.txt` file
5. Text content included in AI context (max 5000 chars per file)
