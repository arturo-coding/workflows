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
```

## ⚡ Detailed Step-by-Step

### 1️⃣ Phase 1: Inception & Context Sync
* **The Problem:** Manually finding tickets, creating branches with correct naming conventions, and moving Jira tickets takes time.
* **The Solution:**
    * Developer prompts: "Start ticket PROJ-101".
    * **Jira MCP:** Updates status to `In Progress` and assigns the ticket to the user.
    * **Git MCP:** Creates `feature/PROJ-101-ticket-title` from `main`.
    * **Context:** The Agent creates a temporary `specs.md` file in memory with the Jira description to guide code generation.

### 2️⃣ Phase 2: Coding & Automated Review Request
* **The Problem:** Writing PR descriptions is tedious, and developers often forget to notify the team.
* **The Solution:**
    * Developer prompts: "Create PR".
    * **Agent:** Generates a PR description summarizing the code changes against the Jira requirements.
    * **Google Chat MCP:** Sends a formatted card to the team's space with the PR link, asking for reviews.

### 3️⃣ Phase 3: QA Deployment (The "Main" Branch)
* **Strategy:** We treat `main` as the source of truth for the QA/Staging environment.
* **Automation:**
    * When the PR receives the required approvals (e.g., 2) and is merged:
    * **GitHub Actions** triggers a Cloud Build pipeline.
    * **Cloud Build** creates the container and updates the **QA Cloud Run Service**.

### 4️⃣ Phase 4: Production Release (The Tag)
* **Strategy:** Production is protected. It does not change with every merge. It changes only with a specific **Version Tag**.
* **Automation:**
    * Developer/Lead prompts: "Create Release v1.2.0".
    * **Agent:** Calls GitHub API to create a Release/Tag on the current commit of `main`.
    * **GitHub Actions** detects the tag pattern (`v*`).
    * **Cloud Build** triggers the **Production Pipeline**, promoting the exact same artifact tested in QA to the Production environment.
