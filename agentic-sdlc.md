# Agentic SDLC: The Context-Driven Pipeline

**Architecture:** Model Context Protocol (MCP) + Event-Driven CI/CD
**Strategy:** Trunk-Based for QA / Tags for Production
**Control Plane:** AI-Powered Code Editor

## 📖 Workflow Overview

This document outlines a high-velocity delivery lifecycle where the **Code Editor acts as the Orchestrator**. By using MCP Servers, we eliminate context switching between Jira, GitHub, and Cloud Consoles.

### Key Lifecycle Rules
1.  **Jira Automation:** Statuses move automatically based on developer intent (`To Do` → `In Progress`).
2.  **Collaboration:** Pull Requests trigger instant notifications in Google Chat.
3.  **Environment Strategy:**
    * **QA/Staging:** Deploys automatically on every merge to the `main` branch.
    * **Production:** Deploys only when a **Release Tag** (e.g., `v1.0.0`) is created.

---

## 🧩 The Agentic Flow Diagram

The following diagram visualizes the timeline of a feature from inception to production, highlighting the interaction between the Developer, the AI Agent, and External Infrastructure.

```text
+--------+            +---------------------+            +-------------------------+
|  USER  |            |   EDITOR (AGENT)    |            |   EXTERNAL SERVICES     |
| (Dev)  |            |    + MCP Client     |            | (Jira, GH, Chat, GCP)   |
+---+----+            +----------+----------+            +------------+------------+
    |                            |                                    |
    |                            |                                    |
    |  PHASE 1: INCEPTION        |                                    |
    |  (Context Loading)         |                                    |
    +--- "Start working on ----> |                                    |
    |     ticket PROJ-101"       | --(MCP: Get Ticket Details)------> | [Jira Cloud]
    |                            |                                    |
    |                            | --(MCP: Update Status)-----------> | [Jira Cloud]
    |                            |    (Set: "To Do" -> "In Progress") |
    |                            |                                    |
    |                            | --(MCP: Git Checkout)------------> | [Local Git]
    |                            |    (Create: feature/PROJ-101)      |
    |                            |                                    |
    |                            |                                    |
    |  PHASE 2: CODE & REVIEW    |                                    |
    |  (Automated Handoff)       |                                    |
    +--- "Code ready. Create --> |                                    |
    |     PR and notify team"    | --(MCP: Git Push)----------------> | [GitHub Remote]
    |                            |                                    |
    |                            | --(MCP: Create PR)---------------> | [GitHub API]
    |                            |    (Desc: Generated from Context)  |
    |                            |                                    |
    |                            | --(MCP: Send Notification)-------> | [Google Chat]
    |                            |    (Msg: "🚨 Review PR #42")       |
    |                            |                                    |
    |                            |                                    |
    |  PHASE 3: QA DEPLOY        |                                    |
    |  (Continuous Delivery)     |                                    |
    |       (Wait for            |                                    |
    |        Approvals)          | <--(Event: PR Merged to Main)----- | [GitHub Actions]
    |                            |                                    |       |
    |                            |                                    |       v
    |                            |                                    | [GCP Cloud Build]
    |                            |          (Auto-Deploy: QA) <-------+-------+
    |                            |                                    |
    |                            |                                    |
    |  PHASE 4: PROD RELEASE     |                                    |
    |  (Controlled Gate)         |                                    |
    +--- "QA Verified. --------> |                                    |
    |     Ship Version v1.2"     | --(MCP: Create Release/Tag)------> | [GitHub API]
    |                            |    (Tag: v1.2.0 Created)           |
    |                            |                                    |
    |                            | <--(Event: Tag Pushed)------------ | [GitHub Actions]
    |                            |                                    |       |
    |                            |                                    |       v
    |                            |                                    | [GCP Cloud Build]
    |                            |        (Auto-Deploy: PROD) <-------+-------+
    v                            v                                    v
