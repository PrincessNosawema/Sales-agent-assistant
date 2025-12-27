# Lead Analysis & Priority Alert System (n8n + Groq + HubSpot)

## 📌 Project Overview
This project is an enterprise-grade lead processing pipeline designed to automate the transition from "Raw Data" to "Actionable Sales Intelligence." It ingests leads from external sources, performs advanced data sanitization via JavaScript, utilizes LLMs (Llama 3.3 via Groq) for sentiment and intent analysis, and orchestrates a multi-channel synchronization strategy involving HubSpot CRM and Slack.

**The Goal:** Eliminate manual lead triaging by providing sales teams with a weighted "Composite Score" and instant priority alerts.

## 🎬 Demo

[Add your demo video/GIF here]

## 🏗️ System Architecture

![System Architecture Diagram](./assets/architecture.png)

## 📸 Workflow Screenshot

![Workflow Screenshot](./assets/workflow-screenshot.png)

## 🛠️ Technical Architecture

### 1. Data Ingestion & Sanitization (The "Safe-Guard" Layer)
 * **Source:** Google Sheets API.
 * **Logic:** A custom JavaScript Normalization Node validates email formats against a comprehensive TLD list and cleans phone records.
 * **Resiliency:** It auto-generates placeholder emails using a unique identifier logic to ensure HubSpot API compatibility even when source data is "dirty."

### 2. AI Intelligence (The "Analyst" Layer)
 * **Model:** Groq (Llama-3.3-70b-versatile).
 * **Process:** The workflow passes the call transcript, industry, and company size to the LLM.
 * **Output:** Extracts structured JSON containing buying_intent_score, urgency_level, pain_points, and next_best_action.

### 3. Composite Scoring Algorithm
I implemented a weighted scoring logic to ensure the AI's opinion is balanced with hard data:

 * **ICP Fit:** Calculated via JS based on industry (SaaS/Finance/Tech) and company size.

### 4. CRM Orchestration & Alerting
 * **Dynamic Routing:** Leads are branched into High, Medium, and Low intent streams.
 * **HubSpot Upsert:** Syncs leads with enriched custom properties (ai_lead_score, intent_tag, analysis_notes).
 * **Instant Slack Alerts:** High and Medium priority leads trigger formatted Slack notifications with specific "Next Steps" for the sales team.
 * **Batch Management:** The system uses a Split-in-Batches (Size: 5) approach to respect Groq and HubSpot rate limits.

## 🚀 Key Features & Highlights
 * **Fail-Safe Logic:** Includes error handling that assigns default scores and "Manual Review" tags if the AI analysis fails.
 * **State Restoration:** Uses "Restore Data" nodes to ensure lead context is preserved after Slack/CRM interactions for the final reporting phase.
 * **Automated Reporting:** Generates a final JSON summary report of the entire batch execution (Average score, success vs. failure rates).

## 💻 Tech Stack
 * **Automation:** n8n
 * **AI/LLM:** Groq (Llama-3.3-70b)
 * **Languages:** JavaScript (Node.js)
 * **Tools:** HubSpot API, Slack API, Google Sheets API

## ⚙️ Setup Instructions
 * **Import Workflow:** Download the Lead_Analysis_System.json and import it into your n8n instance.
 * **Credentials:**
   * Set up Google Sheets OAuth2.
   * Set up Groq API (obtain key from groq.com).
   * Set up HubSpot OAuth2.
   * Set up Slack OAuth2.
 * **Environment Variables:** Ensure your HubSpot account has the following custom properties: ai_lead_score, intent_tag, analysis_notes, and email_status.

## 📈 Business Impact
 * **Reduced Triage Time:** Automates the 5–10 minutes a human takes to read a transcript and score a lead.
 * **Higher Accuracy:** Eliminates subjective bias in lead scoring through a standardized weighted algorithm.
 * **Speed to Lead:** High-intent prospects are flagged in Slack within seconds of ingestion.