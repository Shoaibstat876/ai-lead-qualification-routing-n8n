# AI Lead Qualification & Routing Automation

I built this as a portfolio project using **n8n** to demonstrate AI workflow automation, API integration, lead validation, CRM logging, and conditional routing.

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

## Features

* Webhook-based lead intake
* Email and phone validation
* Error logging for invalid leads
* AI-powered lead classification
* Structured JSON parsing from OpenAI response
* Google Sheets CRM logging
* Hot/Warm lead routing
* Gmail notification for qualified leads
* End-to-end test payloads

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
Hot or Warm → Send Gmail notification
Cold → Log to Leads tab only
Missing email or phone → Log to Errors tab
```

## Tested Scenarios

| Test Case     | Expected Result                         | Status |
| ------------- | --------------------------------------- | ------ |
| Hot lead      | Saved to Leads tab + Gmail notification | Passed |
| Cold lead     | Saved to Leads tab only                 | Passed |
| Missing email | Saved to Errors tab                     | Passed |
| Missing phone | Saved to Errors tab                     | Passed |

## Proof Screenshots

Screenshots are stored in the proof folder and include:

* Full n8n workflow canvas
* Webhook node settings
* Email/phone validation node
* OpenAI HTTP Request settings
* OpenAI JSON parsing Code node
* Google Sheets Leads proof
* Google Sheets Errors proof
* Gmail notification proof

## Important Notes

This is a portfolio project, not a client or production deployment.

I used test lead data only. API keys, credentials, OAuth details, and private webhook secrets are not included in this repository.

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

## Future Improvements

* Add CRM integration such as HubSpot or Airtable
* Add deduplication by email/phone
* Add retry/error handling for failed API calls
* Add Slack or Discord alerts
* Add dashboard reporting for lead quality trends
* Add authentication for webhook access
