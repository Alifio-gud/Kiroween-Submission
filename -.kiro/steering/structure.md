---
inclusion: always
---

# Project Structure

## Main Application Files

- `iframeindex.html` - Main entry point with three-column layout (SDK, Witch, Sidebar)
- `chat-iframe.html` - Isolated chat interface embedded in witch image
- `rightbar.html` - Right sidebar with file uploads and shared thoughts
- `hackathon.html` - LintasEdu SDK loader (video call functionality)
- `combined_server.py` - Unified HTTP + WebSocket server
- `style.css` - Global styles for main layout

## Client Application (clientapp/)

- `clientapp/client.html` - Standalone client with same layout as main app
- `clientapp/client.js` - Client-side logic (WebSocket, HTTP requests)
- `clientapp/README.md` - Client documentation

**Important**: Client layout MUST match iframeindex.html exactly:
- Three-column absolute positioning
- Left: SDK (33.33vw), Center: Witch (33.33vw), Right: Sidebar (400px)
- Chat overlays witch image at bottom (400px height)
- Right sidebar: Offerings (30vh) + Thoughts (60vh)

## Server Files

- `combined_server.py` - Production server (HTTP + WebSocket combined)
- `requirements.txt` - Python dependencies (google-genai, websockets, pypdf)

## Assets

- `witch asset.jpg` - Main witch image displayed in center panel
- `scarymusic.mp3` - Background audio (main app only)

## Data Files (Generated at Runtime)

- `messages.json` - Server-side chat history (shared across all clients)
- `file_context.json` - Uploaded documents metadata for AI context
- `shared_thoughts.json` - Shared page thoughts from all users
- `uploaded_files/` - Directory containing uploaded PDFs and extracted text files

## Archive Folders

- `checkpoint/` - Backup of working files
- `unused/` - Deprecated code (old server implementations, service workers, etc.)

## Architecture

### Main Page (iframeindex.html)
- Three-column absolute positioning layout
- Left: SDK in Shadow DOM (isolation)
- Center: Witch image with chat iframe overlay
- Right: Rightbar iframe (file uploads + shared thoughts)

### Client App (clientapp/client.html)
- Identical layout to main page
- Self-contained HTML/CSS/JS (no iframes except SDK)
- Connects to same server endpoints
- All data shared with main app and other clients

### Chat System (chat-iframe.html)
- Completely isolated from main page
- WebSocket connection to port 8765
- Manages own chat UI and localStorage
- No dependencies on parent page

### Right Sidebar (rightbar.html)
- File upload system with PDF text extraction
- Shared thoughts chat (polls server every 2 seconds)
- Isolated in iframe on main page
- Inline on client app

### SDK System (hackathon.html)
- Loads external LintasEdu SDK
- Isolated in iframe to prevent page interference
- Triggers browser warnings (external SDK behavior)

## Code Organization

- **No build system** - Direct file serving
- **No bundler** - All scripts inline or directly loaded
- **No CSS preprocessor** - Plain CSS
- **No package.json** - Python-only backend

## Conventions

- Use `contenteditable` divs instead of input fields to avoid form submission warnings
- All chat communication via WebSocket (no HTTP POST for messages)
- Shared thoughts use HTTP polling (GET/POST every 2 seconds)
- File uploads use HTTP POST with base64 encoding for PDFs
- localStorage keys: `chatHistory`, `uploadedFiles`
- Server ports: 5000 (HTTP), 8765 (WebSocket)

## Multi-Client Architecture

All clients (main app and client app) share:
- Chat messages (via WebSocket + messages.json)
- Shared thoughts (via HTTP polling + shared_thoughts.json)
- Uploaded files (via HTTP + file_context.json)
- AI context includes all uploaded files and last 10 shared thoughts
