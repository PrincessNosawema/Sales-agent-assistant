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
# 🛠️ The AI Toolbox: 9 Specialized Agent Tools

The core of this system is its ability to autonomously select and execute the right tool for the job. Each tool is engineered with specific logic to handle real-world edge cases.

---

## 📧 Email Management (Gmail)

### 1. Gmail Search & Read
Advanced filtering for unread messages, specific senders, or date ranges. It automatically cleans HTML tags to provide the AI with readable context while preserving attachment metadata.

### 2. Gmail Send New Email
Supports full HTML body construction. It follows a strict Two-Stage Confirmation pattern: drafting a plain-text preview for the user first, then sending only upon a "Yes" confirmation.

### 3. Gmail Reply to Email
Context-aware replying using messageId tracking to maintain thread integrity within the user's inbox.

### 4. Gmail Delete Email (Permanent)
A high-stakes tool built with a critical safety guardrail that requires explicit user confirmation to prevent accidental data loss.

---

## 📅 Calendar Orchestration (Google Calendar)

### 1. Calendar View Schedule
Retrieves events with timezone-aware logic (WAT/UTC+1). Used by the agent to perform "Conflict Checks" before suggesting any new appointments.

### 2. Calendar Create Event
Handles complex scheduling including attendees, locations, and descriptions. It automatically converts natural language (e.g., "Next Tuesday at 2pm") into ISO 8601 timestamps.

### 3. Calendar Update Event
Modifies existing meetings (changing times, adding attendees) and triggers automatic Google Calendar notifications to all participants.

### 4. Calendar Delete Event
Manages cancellations for both single and recurring events with a confirmation loop.

---

## 👤 Identity & CRM (Google Sheets)

### 1. Contact Lookup
A custom-built CRM tool using Google Sheets. It performs fuzzy matching by name, email, or phone. If the user says "Email John," the agent uses this tool to find John's address before calling the Gmail tool.
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