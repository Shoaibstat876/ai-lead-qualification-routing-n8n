# AI Lead Qualification & Routing Automation — Detailed Video Walkthrough Script

## Video Goal

In this video, I will demonstrate my portfolio project: **AI Lead Qualification & Routing Automation**, built using **n8n**.

This project shows how a business can receive lead data from a form or webhook, validate the lead, classify the lead using AI, store the result in Google Sheets, and send an email notification when the lead is Hot or Warm.

---

## 1. Introduction

Hi, my name is **Muhammad Shoaib Abdul Shakoor**.

In this video, I will walk through my portfolio project called **AI Lead Qualification & Routing Automation**.

I built this as a portfolio project using **n8n** to demonstrate practical AI automation skills, including webhook intake, validation logic, OpenAI API integration, JSON parsing, Google Sheets logging, conditional routing, and Gmail notification.

This is not a client or production deployment. It is a demo-ready portfolio project built to show how I can design and test an AI automation workflow.

---

## 2. Problem This Project Solves

Many businesses receive leads from ads, websites, or contact forms.

The problem is that not every lead has the same value.

Some leads are urgent and high-budget. Some are casual inquiries. Some are incomplete because they are missing an email or phone number.

This workflow solves that by automatically checking the lead, classifying it with AI, logging it into a CRM-style Google Sheet, and notifying the team only when the lead is Hot or Warm.

So the main business value is:

* Faster lead qualification
* Less manual checking
* Clean lead records
* Better follow-up priority
* Automatic notification for serious leads

---

## 3. Workflow Overview

Here is the full workflow inside n8n.

The flow is:

Webhook Lead Intake
→ Check Email and Phone
→ If email or phone is missing, write to the Errors sheet
→ If the lead is valid, send the data to OpenAI
→ Parse the AI JSON result
→ Write the lead to Google Sheets
→ Check whether the score is Hot or Warm
→ If Hot or Warm, send a Gmail notification
→ If Cold, only log the lead without notification

This keeps the workflow clean because invalid leads do not go to AI, and Cold leads do not create unnecessary notifications.

---

## 4. Step 1 — Webhook Lead Intake

The first node is **01_Webhook_Lead_Intake**.

This node receives lead data using a POST request.

The expected lead fields are:

* name
* email
* phone
* service_needed
* budget
* urgency
* message

For testing, I used PowerShell to send JSON payloads to the webhook.

In a real use case, this webhook could be connected to a website form, landing page, CRM form, Facebook lead form integration, or any external system that can send JSON data.

---

## 5. Step 2 — Email and Phone Validation

The second node is **02_Check_Email_And_Phone**.

This is an IF node.

It checks whether the lead is missing an email or missing a phone number.

The logic is:

If email is empty OR phone is empty, then the lead is invalid.

The invalid lead goes to the error branch.

The valid lead goes to the OpenAI classification branch.

This is important because we should not waste AI/API calls on incomplete leads. It also protects the workflow from bad data.

---

## 6. Step 3 — Error Logging

If a lead is missing email or phone, the workflow goes to **03_Write_Error_To_Sheet**.

This node writes the invalid lead into the **Errors** tab in Google Sheets.

The Errors tab has these columns:

* timestamp
* raw_input
* error_reason

For example:

If email is missing, the error reason is **Missing email**.

If phone is missing, the error reason is **Missing phone**.

This is useful because bad leads are not lost. They are stored separately for review.

---

## 7. Step 4 — OpenAI Lead Classification

If the lead is valid, it goes to **04_OpenAI_Classify_Lead**.

This is an HTTP Request node that calls the OpenAI Chat Completions API.

I used **GPT-4o-mini** for classification.

The prompt asks the AI to classify the lead and return strict JSON only.

The required JSON keys are:

* score
* pain_point
* urgency_level
* next_action
* follow_up_message

The score must be one of:

* Hot
* Warm
* Cold

The urgency level must be one of:

* High
* Medium
* Low

This step converts unstructured lead information into structured business data.

---

## 8. Step 5 — Parse OpenAI JSON

After OpenAI returns the response, the workflow goes to **05_Parse_OpenAI_JSON**.

This is a Code node.

The purpose of this node is to safely extract and parse the AI response from:

choices[0].message.content

Then it converts the AI JSON string into clean fields that the next nodes can use.

The parsed fields include:

* ai_score
* pain_point
* urgency_level
* next_action
* follow_up_message
* openai_raw_content

I also added basic error handling. If the OpenAI response is missing or not valid JSON, the Code node throws a clear error.

This makes the workflow easier to debug.

---

## 9. Step 6 — Write Valid Lead to Google Sheets

After parsing the AI result, the workflow goes to **06_Write_Lead_To_Sheet**.

This node appends the lead into the **Leads** tab in Google Sheets.

The Leads tab works like a simple CRM.

It stores:

* timestamp
* name
* email
* phone
* service_needed
* budget
* urgency
* score
* pain_point
* urgency_level
* next_action
* follow_up_message
* status

The status is set to **Logged**.

This gives a clean record of every valid lead, whether it is Hot, Warm, or Cold.

---

## 10. Step 7 — Hot/Warm Routing

After the lead is stored, the workflow goes to **07_Check_Hot_Or_Warm**.

This IF node checks the AI score.

The logic is:

If score equals Hot OR score equals Warm, then send a notification.

If the score is Cold, the workflow stops after logging the lead.

This is useful because only qualified leads create alerts.

Cold leads are still saved, but they do not interrupt the team.

---

## 11. Step 8 — Gmail Notification

For Hot or Warm leads, the workflow goes to **08_Send_Gmail_Notification**.

This node sends a Gmail email notification.

The notification includes:

* Lead score
* Name
* Email
* Phone
* Service needed
* Budget
* Urgency
* Pain point
* Urgency level
* Next action
* Suggested follow-up message

This means a sales or business team can quickly understand why the lead is important and what to do next.

For example, if the lead is Hot, the email subject becomes:

New Hot Lead — Usman Farooq

---

## 12. Google Sheets Proof

Here is the Google Sheet used for this project.

It has two tabs:

**Leads** and **Errors**.

In the Leads tab, I tested both Hot and Cold leads.

The Hot lead was saved and also triggered a Gmail notification.

The Cold lead was saved only, without sending a Gmail alert.

In the Errors tab, I tested missing email and missing phone cases.

Both invalid leads were correctly saved with the right error reason.

---

## 13. Test Scenarios

I tested four main scenarios.

First, a Hot lead:

The lead had urgent intent, a higher budget, and a clear need for automation.
The result was saved to the Leads tab and a Gmail notification was sent.

Second, a Cold lead:

The lead had low urgency and a low budget.
The result was saved to the Leads tab only. No Gmail notification was sent.

Third, a missing email test:

The workflow detected the missing email and saved the record to the Errors tab.

Fourth, a missing phone test:

The workflow detected the missing phone and saved the record to the Errors tab.

So the validation, AI classification, routing, logging, and notification behavior were all tested.

---

## 14. GitHub Documentation

I also documented this project on GitHub.

The repository includes:

* A professional README
* A sanitized n8n workflow example JSON
* Proof screenshots
* PowerShell test payloads

The workflow JSON is sanitized for public sharing.

Private values like API keys, OAuth credentials, Google Sheet IDs, webhook secrets, and personal email addresses are not included.

The README also clearly states that this is a portfolio project, not a client or production deployment.

---

## 15. Skills Demonstrated

This project demonstrates:

* n8n workflow building
* Webhook automation
* Input validation
* Error branch handling
* OpenAI API integration
* Prompt design for structured JSON output
* JavaScript parsing inside n8n
* Google Sheets CRM logging
* Conditional routing
* Gmail automation
* End-to-end workflow testing
* Safe GitHub documentation

---

## 16. Future Improvements

If I improve this project further, I would add:

* Webhook authentication
* Retry handling for failed API calls
* Lead deduplication by email or phone
* CRM integration such as HubSpot or Airtable
* Slack or Discord alerts
* Dashboard reporting for lead quality trends
* More advanced scoring rules
* Human review step for borderline leads

---

## 17. Closing

That is the full walkthrough of my **AI Lead Qualification & Routing Automation** project.

I built this as a portfolio project using n8n to show practical AI automation skills.

The workflow receives lead data, validates required fields, classifies valid leads using OpenAI, logs results into Google Sheets, routes based on Hot, Warm, or Cold status, and sends Gmail notifications for qualified leads.

Thank you for watching.
