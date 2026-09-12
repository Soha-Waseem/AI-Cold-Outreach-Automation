# AI Cold Outreach Automation

### Automated, personalized B2B outreach built with n8n + Google Gemini

## Overview

**AI Cold Outreach Automation** is an end-to-end workflow that turns a list of leads in Google Sheets into personalized cold outreach emails, sent automatically on a weekly schedule.

Instead of sending the same generic template to every contact, an AI Agent writes a unique, context-aware email for each lead based on their name, business, category, and city, then formats it, attaches a portfolio PDF, sends it through Gmail, and logs the result back into the sheet.

The workflow is built in **n8n**, with **Google Gemini** powering the AI Agent and a **Structured Output Parser** enforcing a consistent subject/body format.

---

## Why I Built This

Cold outreach at scale usually means one of two things: generic templates that get ignored, or hours spent manually personalizing every email.

I wanted a system that could do the personalization automatically, at a professional, executive tone, without losing the human feel of a real note written to that specific business.

This project also explores how **AI agents can be given strict brand voice and formatting rules** (tone, structure, fallback logic, banned characters) so the output is production-ready, not just a rough draft that needs editing every time.

---

## What It Does

Every week, the workflow:

| # | Step | What Happens |
|---|------|---------------|
| 01 | **Trigger** | Runs automatically on a weekly schedule |
| 02 | **Fetch Leads** | Pulls contact and business details from Google Sheets |
| 03 | **Generate Email** | AI Agent writes a personalized subject and body using Google Gemini |
| 04 | **Parse Output** | Structured Output Parser enforces a clean Subject/Body format |
| 05 | **Format Email** | JavaScript node converts markdown to clean HTML for Gmail |
| 06 | **Attach Portfolio** | Downloads a portfolio PDF from Google Drive |
| 07 | **Send Email** | Sends the personalized email via Gmail with the attachment |
| 08 | **Log Result** | Updates the Google Sheet with send status and email content |

---

# AI Agent

The AI Agent uses a detailed system prompt that defines its role, tone, and output rules precisely, rather than relying on generic instructions.

### Role

The agent writes on behalf of an AI Solutions Engineer offering custom AI agent development services to e-commerce brands.

### Rules It Follows

- Uses fallback values when lead data (name, business, city) is missing
- Includes exactly two quantified value propositions per email (e.g. resolving up to 70% of routine inquiries, lifting conversion rates 15 to 20%)
- Enforces paragraph spacing and clean bullet formatting
- Avoids dashes entirely, using commas, periods, or parentheses instead
- Closes every email with a full signature block and a portfolio link

### Response Philosophy

The agent is designed to be:

- **Personalized**
- **Professional**
- **Consultative**
- **Consistent**
- **Metric-driven**

It is instructed never to sound like a generic template, and to keep the tone confident without being pushy.

---

# Workflow Architecture
          ┌──────────────────────┐
          │   Schedule Trigger    │
          │   (Weekly, n8n)       │
          └───────────┬───────────┘
                      │
                      ▼
          ┌──────────────────────┐
          │  Get Data from Sheet  │
          │   (Google Sheets)     │
          └───────────┬───────────┘
                      │
                      ▼
          ┌──────────────────────┐
          │       AI Agent        │
          │  Cold Email Writer    │
          └───────────┬───────────┘
                      │
          ┌──────────────────────┐
          │   Google Gemini       │ 
          │     Chat Model        │
          └───────────┬───────────┘
                      │
                      ▼
          ┌──────────────────────┐
          │   Structured Output   │
          │        Parser         │
          └───────────┬───────────┘
                      │
                      ▼
          ┌──────────────────────┐
          │ Code in JavaScript    │
          | (Markdown → HTML)     │
          └───────────┬───────────┘
                      │
          ┌──────────────────────┐
          │ Download File         │
          │ (Portfolio, Drive)    │
          └───────────┬───────────┘
                      │
                      ▼
          ┌──────────────────────┐
          │    Send a Message     │
          │       (Gmail)         │
          └───────────┬───────────┘
                      │
                      ▼
          ┌──────────────────────┐
          │ Update Row in Sheet   │
          │ (Google Sheets)       │
          └───────────┬───────────┘


The exported workflow contains all nodes above with their corresponding connections.

---

# Tech Stack

| Technology | Role |
|------------|------|
| **n8n** | Workflow automation |
| **n8n AI Agent** | Agent orchestration |
| **Google Gemini** | Large language model for email generation |
| **Structured Output Parser** | Enforces consistent Subject/Body format |
| **Google Sheets** | Lead source and outreach log |
| **Google Drive** | Portfolio attachment storage |
| **Gmail** | Email delivery |
| **JavaScript (Code Node)** | Markdown-to-HTML formatting |

---

# How It Works

### 01 — Weekly trigger fires

The Schedule Trigger runs automatically once a week to start the outreach cycle.

### 02 — Lead data is pulled

The workflow reads contact name, business name, category, city, and email from a connected Google Sheet.

### 03 — AI Agent writes the email

Google Gemini generates a personalized subject line and body using the lead's details, following the agent's tone, formatting, and value-proposition rules.

### 04 — Output is structured

The Structured Output Parser ensures the AI response always returns a clean Subject and Body field, ready to use downstream.

### 05 — Email is formatted

A JavaScript node converts markdown formatting (bold, bullets, links) into clean HTML suitable for Gmail.

### 06 — Portfolio is attached

The workflow downloads a portfolio PDF from Google Drive to include as an attachment.

### 07 — Email is sent

Gmail sends the personalized, formatted email with the attachment to the lead.

### 08 — Sheet is updated

The Google Sheet is updated with the send status, subject, and body, keeping a full outreach log.


---

# Getting Started

## Prerequisites

You'll need:

- [n8n](https://n8n.io/)
- Google Gemini API credentials
- Google Sheets OAuth2 credentials
- Google Drive OAuth2 credentials
- Gmail OAuth2 credentials
- A Google Sheet with columns for Name, Email, Business Name, Category, and City
- A portfolio PDF stored in Google Drive

## Import the Workflow

1. Open your n8n instance.
2. Create or open your workspace.
3. Select **Import from File**.
4. Import the workflow JSON file from this repository.
5. Configure your Google Sheets, Google Drive, Gmail, and Google Gemini credentials.
6. Update the Sheet and Drive file references to point to your own lead sheet and portfolio file.
7. Save the workflow.
8. Activate/test the workflow.

---

# Credentials

The exported workflow requires Google Sheets, Google Drive, Gmail, and Google Gemini credentials to run.

**Never commit your API keys, OAuth tokens, or email addresses to GitHub.**

Use n8n's credential management system and keep sensitive credentials outside the repository.

---

# Use Cases

AI Cold Outreach Automation can be useful for:

- Freelancers and agencies doing B2B outreach
- AI/automation consultants pitching custom solutions
- Sales teams personalizing outreach at scale
- Founders running early customer acquisition
- Anyone maintaining a lead list who wants consistent, personalized follow-up

---

# Future Improvements

Potential next iterations could include:

- A/B testing different email tones or structures
- Reply detection and automatic follow-up sequencing
- CRM integration instead of Google Sheets
- Lead scoring before outreach
- Multi-channel outreach (LinkedIn + email)
- Analytics dashboard for open/response rates

These are **future possibilities**, not features currently implemented in the workflow.

---

# Project Status

**Functional Prototype**

Current workflow components:

- ✅ Weekly Schedule Trigger
- ✅ Google Sheets lead source
- ✅ AI Agent email generation
- ✅ Google Gemini integration
- ✅ Structured Output Parser
- ✅ Markdown-to-HTML formatting
- ✅ Google Drive attachment
- ✅ Gmail delivery
- ✅ Automated outreach logging

The workflow itself is currently marked inactive in the exported configuration, so activation should be done after importing and configuring credentials.

---

# What This Project Demonstrates

This project demonstrates practical experience rather than sending generic templates, the workflow uses **defined agent behavior + structured output + multi-service automation** to deliver personalized outreach at scale.

