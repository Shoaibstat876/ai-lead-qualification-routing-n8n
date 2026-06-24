# AI Lead Qualification & Routing Automation

I built this as a portfolio project using **n8n** to demonstrate AI workflow automation, API integration, lead validation, CRM logging, AI classification, and conditional routing.

The workflow receives lead data through a webhook, validates required contact fields, classifies valid leads using OpenAI, stores results in Google Sheets, and sends a Gmail notification for Hot/Warm leads.

## Workflow Overview

```text
Webhook Lead Intake
→ Check Email & Phone
→ If Missing Email/Phone: Write to Errors Sheet
→ If Valid: OpenAI Lead Classification
→ Parse OpenAI JSON Result
→ Write Lead to Google Sheets
→ Check Hot/Warm vs Cold
→ Send Gmail Notification for Hot/Warm Leads
```

## Tech Stack

* n8n Cloud
* OpenAI GPT-4o-mini via HTTP Request
* Google Sheets
* Gmail
* PowerShell for webhook testing
* GitHub for documentation

## Repository Structure

```text
ai-lead-qualification-routing-n8n/
├── README.md
├── workflow/
│   └── ai-lead-qualification-routing-n8n-workflow.example.json
├── screenshots/
│   ├── 01-full-n8n-workflow-canvas.png
│   ├── 03-email-phone-validation-if-node.png
│   ├── 04a-openai-http-request-url-settings.png
│   ├── 04b-openai-json-body-settings.png
│   ├── 05-parse-openai-json-code-node.png
│   ├── 06-leads-tab-hot-cold-proof.png
│   ├── 07-errors-tab-validation-proof.png
│   └── 08-gmail-hot-lead-notification-proof.png
└── test-payloads/
    └── test-payloads.txt
```

## Features

* Webhook-based lead intake
* Email and phone validation
* Error logging for invalid leads
* AI-powered lead classification
* Structured JSON parsing from OpenAI response
* Google Sheets CRM-style logging
* Hot/Warm lead routing
* Gmail notification for qualified leads
* End-to-end testing with multiple payloads
* Sanitized n8n workflow example included

## Lead Input Fields

```text
name
email
phone
service_needed
budget
urgency
message
```

## Google Sheets Structure

### Leads Tab

```text
timestamp
name
email
phone
service_needed
budget
urgency
score
pain_point
urgency_level
next_action
follow_up_message
status
```

### Errors Tab

```text
timestamp
raw_input
error_reason
```

## AI Classification Output

The OpenAI step returns structured JSON with:

```text
score
pain_point
urgency_level
next_action
follow_up_message
```

Example output:

```json
{
  "score": "Hot",
  "pain_point": "Need for quick lead qualification and alert system for serious buyers",
  "urgency_level": "High",
  "next_action": "Schedule a demo for this week",
  "follow_up_message": "Hi Usman, we understand your urgency and would like to schedule a demo..."
}
```

## Routing Logic

```text
Hot or Warm → Save to Leads tab + send Gmail notification
Cold → Save to Leads tab only
Missing email or phone → Save to Errors tab
```

## Tested Scenarios

| Test Case     | Expected Result                              | Status |
| ------------- | -------------------------------------------- | ------ |
| Hot lead      | Saved to Leads tab + Gmail notification sent | Passed |
| Cold lead     | Saved to Leads tab only                      | Passed |
| Missing email | Saved to Errors tab                          | Passed |
| Missing phone | Saved to Errors tab                          | Passed |

## Proof Screenshots

The `screenshots/` folder includes:

* Full n8n workflow canvas
* Email/phone validation node
* OpenAI HTTP Request URL/settings
* OpenAI JSON body settings
* OpenAI JSON parsing Code node
* Google Sheets Leads proof
* Google Sheets Errors proof
* Gmail notification proof

## Workflow Example

A sanitized n8n workflow export is included here:

```text
workflow/ai-lead-qualification-routing-n8n-workflow.example.json
```

This file is provided as a safe example for portfolio review. Private values such as API keys, credential IDs, Google Sheet IDs, webhook secrets, and personal email addresses were replaced with placeholders.

## Test Payloads

PowerShell test payloads are included here:

```text
test-payloads/test-payloads.txt
```

The payloads cover:

* Hot lead
* Cold lead
* Missing email
* Missing phone

## Important Notes

This is a portfolio project, not a client or production deployment.

I used test lead data only. API keys, OAuth secrets, credential tokens, private webhook URLs, and production credentials are not included in this repository.

The workflow JSON in this repository is a sanitized example file. To run it in a real n8n account, the placeholder values must be replaced with valid credentials and Google Sheet configuration.

## What I Practiced

* Building webhook-based automation in n8n
* Validating lead data before AI processing
* Calling OpenAI through an HTTP Request node
* Forcing structured JSON output from an LLM
* Parsing AI output safely with JavaScript
* Writing clean records to Google Sheets
* Routing leads based on AI classification
* Sending automated Gmail alerts
* Testing workflow behavior with multiple payloads
* Preparing safe public documentation for GitHub

## Future Improvements

* Add CRM integration such as HubSpot or Airtable
* Add deduplication by email or phone
* Add retry/error handling for failed API calls
* Add Slack or Discord alerts
* Add webhook authentication
* Add dashboard reporting for lead quality trends
* Add logging for OpenAI/API failures
