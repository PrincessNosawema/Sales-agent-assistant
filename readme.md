# 🚀 Autonomous B2B Lead Qualification & Enrichment Pipeline

An autonomous B2B prospecting engine that scrapes localized business data, uses Generative AI to perform multi-factor lead scoring, and synchronizes enriched data to Airtable for high-ticket sales outreach.
![architecture](Lead_enrichment_architecture.PNG)

## 📌 Overview
Manual lead prospecting is a bottleneck for high-ticket service providers. This project automates the entire "Top of Funnel" (ToFu) process. It moves beyond simple scraping by using an LLM to "read" business health—categorizing leads into Hot, Warm, or Cold based on professional markers (ratings, domain authority, and digital footprint).

## Video Demo
**https://www.loom.com/share/934eea1fbeba4f1f81709532f03c1f55**

## Workflow Screenshot
![Screenshot](Enrichment_workflow_screenshot.png)

## 🛠 Tech Stack
 * Orchestration: Make.com (Advanced Aggregator/Iterator patterns)
 * Data Extraction: Apify (Google Maps Scraper)
 * Intelligence: Google Gemini 2.0 Flash (Structured JSON Output)
 * Database/CRM: Airtable
 * Communications: Gmail API

## 🏗 Workflow Architecture
The pipeline is designed for efficiency and cost-optimization:
 * Trigger: An Apify Actor finishes a Google Maps scrape; a Webhook sends the dataset to the pipeline.
 * Data Processing: * Aggregation: Collects raw dataset items into a consolidated array.
   * Iteration: Loops through each business to process them individually.
 * AI Analysis (The Brain):
   * Uses a custom-engineered prompt to act as an "Automation Consultant."
   * Scoring Logic:
     * HOT: Rating ≥ 4.0, 10+ reviews, professional domain email, active website.
     * WARM/COLD: Based on missing digital infrastructure or low social proof.
   * Strategic Output: Generates a custom outreach strategy and identifies specific business pain points.
 * Storage: Maps 11+ data points (Email, Phone, Industry, Rating, AI Insights) into Airtable.
 * Closing Loop: Once the batch is processed, a summary email is sent to the user with a direct link to the new Airtable records.

## 🧠 Key Automation Engineering Features
 * Structured JSON Output: The Gemini module is configured with a strict JSON response schema to ensure zero-fail parsing into Airtable.
 * Regex Sanitization: Implemented replace() functions to clean LLM markdown artifacts (e.g., stripping ```json tags) before parsing.
 * Batch Efficiency: Uses an Aggregator-to-Iterator pattern to handle bulk data without hitting API rate limits or creating redundant operations.
 * Dynamic Lead Triage: Instead of just "gathering" leads, the system "qualifies" them, saving sales teams hours of manual vetting.

## 🚀 How to Use
 * Import Blueprint: Download the .json file from this repo and import it into your Make.com dashboard.
 * Configure Connections: Connect your Apify API Token, Google AI (Gemini) API Key, and Airtable Base.
 * Airtable Schema: Ensure your Airtable table has fields matching the mapping in Module 11 (Business Name, Industry, Lead Quality, etc.).
 * Run: Trigger an Apify run and watch the leads populate in real-time.