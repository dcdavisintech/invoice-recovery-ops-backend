# Invoice Recovery Ops: AI-Driven Automation Backend

A production-grade, event-driven automation infrastructure built to streamline past-due invoice recovery for independent marketing and creative agencies. This ecosystem leverages advanced AI text generation, structured workflows, and real-time data syncs, completely bounded by a secure human-in-the-loop guardrail.

---

## 🏗️ System Architecture Overview

Instead of a single monolith, this backend utilizes a modular, decoupled architecture composed of three interconnected n8n workflows that mirror the lifecycle of client and invoice data:

### 1. Onboarding & Client Intake (`tally-onboarding-intake.json`)
* **Trigger:** Customer onboarding submission via Tally form.
* **Logic:** Form processing, validation, and schema mapping.
* **Action:** Programmatically provisions and appends the new client profile into the central tracking database.

### 2. Core Invoice Processing & Routing Engine (`invoice-recovery-ops.json`)
* **Trigger:** Chronological database scheduler loop or manual engine execution.
* **Logic:** Evaluates debtor records (days overdue, service type, balance) and filters for actionable targets.
* **AI Layer:** Integrates with the `Google Gemini Chat Model` using custom system instructions to dynamically generate personalized, tone-specific collection text matching individual client preferences.
* **Execution:** Generates the initial context-aware communication draft and queues it for administrative verification.

### 3. 1-Click Interactive Approval Guard (`invoice-recovery-engine.json`)
* **Trigger:** Dedicated HTTP Webhook endpoints (`approve-draft` / `edit-draft`).
* **Security Guardrail:** Instead of emailing a debtor automatically, the system routes a dynamic HTML review module to the operator. 
* **Action:** Secure tokens containing the text, debtor address, and tracking keys are passed via interactive buttons. Clicking "Approve & Send" triggers the production Gmail node for final delivery and updates the database state to `Sent`. Clicking "Request Changes" flags the invoice status for manual revision.

---

## 🛠️ Technology Stack
* **Workflow Automation Engine:** n8n Pro
* **Large Language Model (LLM):** Google Gemini (via native n8n LangChain chat integrations)
* **Database & CRM Integration:** Google Sheets API
* **Communication & Routing:** Gmail API, Custom HTTP Webhooks
