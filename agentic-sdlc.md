# Agentic SDLC: Context-Driven Delivery Pipeline

**Overview:** This document outlines the operational workflow where the **AI-Augmented Editor** (via MCP) acts as the central orchestrator. 

**Key Strategy:** * **Context over Clicks:** Status updates and notifications are side-effects of natural language commands.
* **Environment Strategy:** * `main` branch triggers deployment to **QA/Staging**.
    * `git tag` (Semantic Versioning) triggers deployment to **Production**.

---

## 📐 The Master Workflow (ASCII Flow)

This diagram illustrates the interaction between the Developer, the AI Agent (Editor), and the External Services through the Model Context Protocol (MCP).

```text
+--------+            +---------------------+            +-------------------------+
|  USER  |            |   EDITOR (AGENT)    |            |    EXTERNAL SERVICES    |
| (Dev)  |            |     + MCP Layer     |            | (Jira, GH, G-Chat, GCP) |
+---+----+            +----------+----------+            +------------+------------+
    |                            |                                    |
    |                            |                                    |
    |  PHASE 1: INCEPTION & SYNC |                                    |
    |                            |                                    |
    +--- "Start working on ----> |                                    |
    |     ticket PROJ-101"       | --(MCP: Get Ticket Details)------> | [Jira]
    |                            |                                    |
    |                            | --(MCP: Update Status)-----------> | [Jira]
    |                            |    (Set: "To Do" -> "In Progress") |
    |                            |                                    |
    |                            | --(MCP: Git Checkout)------------> | [Local Git]
    |                            |    (Create: feature/PROJ-101)      |
    |                            |                                    |
    |                            |                                    |
    |  PHASE 2: CODING & REVIEW  |                                    |
    |                            |                                    |
    +--- "Code ready. Create --> |                                    |
    |     PR and notify team"    | --(MCP: Push & Create PR)--------> | [GitHub]
    |                            |    (Desc: Generated from Context)  |
    |                            |                                    |
    |                            | --(MCP: Webhook/Msg)-------------> | [Google Chat]
    |                            |    (Msg: "Please Review PR #42")   |
    |                            |                                    |
    |                            |                                    |
    |  PHASE 3: QA DEPLOYMENT    |                                    |
    |                            |                                    |
    |          (Approvals met)   |                                    |
    |                            | <--(Event: PR Merged to Main)----- | [GitHub Actions]
    |                            |                                    |
    |                            |                                    | [GCP Cloud Build]
    |                            |                                    |       |
    |                            |          (Auto-Deploy: QA) <-------+-------+
    |                            |                                    |
    |                            |                                    |
    |  PHASE 4: PROD RELEASE     |                                    |
    |                            |                                    |
    +--- "QA looks good. ------> |                                    |
    |     Release version 1.2"   | --(MCP: Create Release/Tag)------> | [GitHub]
    |                            |    (Tag: v1.2.0)                   |
    |                            |                                    |
    |                            | <--(Event: New Tag Pushed)-------- | [GCP Cloud Build]
    |                            |                                    |       |
    |                            |       (Auto-Deploy: PROD) <--------+-------+
    |                            |                                    |
    v                            v                                    v
