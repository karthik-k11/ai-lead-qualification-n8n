# AI Lead Qualification & Smart Routing Workflow (n8n)

## Overview

This project implements an AI-powered CRM lead qualification and routing system using n8n.

Incoming leads are received via a webhook, scored using an LLM API (OpenRouter), classified as Hot, Warm, or Cold, and dynamically routed to the appropriate sales category. All processed leads are automatically logged into Google Sheets.

The workflow is deployed in production mode with secure API key protection.

---

## Features

- Webhook-based lead ingestion (POST endpoint)
- AI-driven lead scoring (1–100 scale)
- Lead temperature classification (Hot / Warm / Cold)
- Conditional routing using IF nodes
- Personalized outreach message generation
- Automated Google Sheets logging
- Header-based API key protection for webhook
- Secure credential storage in n8n (no hardcoded secrets)

---

## Workflow Architecture

Webhook  
→ HTTP Request (OpenRouter LLM API)  
→ JSON Parsing  
→ Conditional Routing (IF logic)  
→ Google Sheets (Append Row)

---

## Lead Processing Logic

1. Lead data is received via webhook:
   - name
   - email
   - company
   - role
   - industry
   - budget
   - country

2. The LLM evaluates the lead and returns structured JSON containing:
   - lead_temperature
   - score (1–100)
   - industry_classification
   - priority_reason
   - personalized_message

3. Routing logic:
   - score > 75 → High Priority / Inside Sales
   - score <= 75 → Nurture Campaign

4. Results are appended to Google Sheets with:
   - name
   - email
   - company
   - final_score
   - lead_temperature
   - routing_team

---

## Security Implementation

- Webhook protected using header-based authentication
- OpenRouter API key stored in encrypted n8n credentials
- Google Sheets OAuth authentication
- No secrets committed to repository
- Production webhook endpoint used (no test mode dependency)

---

## Sample Request

POST /webhook/lead-qualification

```json
{
  "name": "Rahul",
  "email": "rahul@test.com",
  "company": "FinTech Labs",
  "role": "CTO",
  "industry": "FinTech",
  "budget": 15000,
  "country": "India"
}
```

## Deployment Steps

1. Import AI Lead Qualification Engine.json into n8n.

2. Configure Custom Auth credential with OpenRouter API key.

3. Configure Google Sheets OAuth credential.

4. Configure Webhook Header Authentication.

5. Publish workflow.

6. Use Production Webhook URL.

## Tech Stack

- n8n

- OpenRouter (LLM API)

- Google Sheets API

- Webhook-based REST architecture
