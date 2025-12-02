---
inclusion: always
---

# Product Overview

Witch Chat Interface - A multi-client AI-powered chat application with a mystical witch theme that integrates the LintasEdu SDK for video communication.

## Core Features

- **Real-time AI chat** using Google Gemini API with context awareness
- **WebSocket-based communication** (no page reloads)
- **LintasEdu SDK integration** for video calls
- **Document upload system** with PDF text extraction for context-aware AI responses
- **Shared thoughts system** - All users can contribute thoughts visible to everyone and the AI
- **Multi-client support** - Multiple users can connect simultaneously and share data
- **Persistent chat history** stored on server
- **Mystical/dark theme** with atmospheric music (main app)

## Applications

### Main Application (iframeindex.html)
- Full-featured interface with iframe isolation
- Background music
- Shadow DOM for SDK isolation
- Intended for primary/host user

### Client Application (clientapp/client.html)
- Standalone client with identical layout
- Connects to same server
- Shares all data with main app and other clients
- Intended for participants/guests

## User Experience

### Layout (Both Apps)
- **Three-column layout**: SDK (left 33.33vw), Witch image (center 33.33vw), Sidebar (right 400px)
- **Chat interface** overlays the witch image at the bottom (400px height)
- **Right sidebar** has two sections:
  - Offerings to the witch (30vh) - File uploads
  - Page Thoughts (60vh) - Shared chat

### Shared Data Across All Clients
- Chat messages with the witch
- Shared page thoughts
- Uploaded documents (PDFs automatically converted to text)
- All data synchronized via server

### AI Context
The AI receives context from:
1. Uploaded documents (up to 2000 chars per file)
2. Last 10 shared thoughts from all users
3. Current user message

## Key Constraint

The LintasEdu SDK triggers browser "Reload site?" warnings due to its built-in session protection. This is external SDK behavior and cannot be modified from this codebase.

## Use Cases

- **Educational sessions** - Teacher uses main app, students use client app
- **Collaborative research** - Multiple researchers share documents and thoughts
- **Group consultations** - Video call + shared AI assistant with document context
- **Remote meetings** - Participants share thoughts and documents with AI facilitation
