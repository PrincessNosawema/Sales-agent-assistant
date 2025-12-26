# 📱 Advanced AI Executive Assistant (n8n + Gemini + WhatsApp)

An autonomous, production-grade AI agent designed to manage Gmail, Google Calendar, and Contacts through a WhatsApp interface. This project goes beyond simple "chatbot" logic, implementing professional-grade features like rate-limiting, error handling, and multi-tool orchestration.

## 🎥 Demo

**[Live Demo Link](#)** • **[n8n Workflow JSON](#)**


### System Architecture
![System Architecture Diagram](path/to/architecture-diagram.png)

### Workflow Screenshot
![n8n Workflow Screenshot](path/to/workflow-screenshot.png)

---
## 🚀 Overview

This system acts as a personal Chief of Staff. By leveraging Gemini 2.5 Flash and the LangChain Agent framework within n8n, the assistant can interpret natural language requests to schedule meetings, draft professional emails, and look up contact information—all while maintaining a conversation history.

### Key Engineering Highlights:
* **Tool Calling & Orchestration**: Uses an Agentic framework to autonomously choose between 9+ tools (Gmail Search, Calendar Create, Contact Lookup, etc.).
* **Safety-First Design**: Implemented a "Two-Stage Confirmation" pattern. The AI cannot send emails or delete events without a secondary "Yes" from the user.
* **Timezone Intelligence**: Native support for Africa/Lagos (WAT) with automatic UTC conversion to ensure calendar accuracy.
* **Production Guardrails**: Includes a custom JavaScript-based Rate Limiter and a global Analytics Logger.

---

## 🛠️ Tech Stack

* **Logic Engine**: n8n (Self-hosted/Cloud)
* **Brain**: Google Gemini 2.5 Flash (via LangChain nodes)
* **Memory**: Window Buffer Memory (Session-aware)
* **Database/CRM**: Google Sheets (Contact Lookup)
* **Communications**: Meta WhatsApp Business API
* **Productivity**: Google Workspace (Gmail & Calendar API)

---

## 🧠 Advanced Features

### 1. Smart Email Management
The agent doesn't just "send" text. It:
* Follows professional communication templates (Casual-Professional, Warm Follow-up, etc.).
* Uses HTML formatting for the final sent email while showing the user a Plain Text preview for readability on mobile.
* Handles threaded replies by retrieving messageId contexts.

### 2. Calendar Conflict Resolution
Before booking any event, the agent is programmed to:
* Check the user's schedule for the requested time.
* If a conflict exists, it proactively suggests 2-3 alternative slots rather than failing the request.

### 3. Resilience & Security
* **Rate Limiting**: A custom Code Node prevents API abuse by limiting user requests per minute.
* **Interactive Parsing**: Handles both standard text messages and WhatsApp Interactive Buttons.
* **Error Handling**: A dedicated error branch ensures the user receives a helpful message if an API call fails, rather than the workflow simply "hanging."

---

## 📋 Workflow Architecture

* **Webhook**: Receives incoming WhatsApp data.
* **Parser**: JavaScript node extracts user intent, phone number, and media type.
* **Rate Limiter**: Validates request frequency.
* **AI Agent**: The core logic hub that processes the request using Gemini.
* **Tool Belt**:
  * Gmail Search: "Find unread emails from my boss."
  * Calendar View: "What does my Tuesday look like?"
  * Contact Lookup: "What is John's email?"
* **Response Formatter**: Converts Markdown/LLM output into WhatsApp-friendly formatting (e.g., converting `**bold**` to `*bold*`).

---

## ⚙️ Setup Instructions

* **n8n Setup**: Import the provided `.json` file into your n8n instance.
* **API Credentials**:
  * Configure the Google Console for Gmail, Calendar, and Sheets.
  * Set up a Meta Developer App for the WhatsApp Business API.
  * Get an API Key from Google AI Studio (Gemini).
* **Database**: Prepare a Google Sheet with headers `Name`, `Email`, `Phone` for the Contact Lookup tool.
* **Environment Variables**: Update the Timezone and Profile Name in the "Advanced AI Agent" system prompt.