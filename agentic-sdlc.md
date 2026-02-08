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
PHASE 1: INCEPTION & TDD (RED)
 (Local / Editor MCP)

+----------------------+  +---------------------------+   +-------------------------+
| DEVELOPER (You)      |  | INTELLIGENT EDITOR        |   | EXTERNAL SERVICES       |
| "Start PROJ-101"     |  | (Antigravity / Cursor)    |   | (Jira, GH, Chat, GCP)   |
+----------+-----------+  +-------------+-------------+   +------------+------------+
           |                            |                              |
           v                            |                              |
[ 1. Context Sync ]                     |                              |
           |                            |                              |
           +--------------------------> | --(MCP: Get Ticket)------> | [Jira Cloud]
                                        | --(MCP: Set In Progress)-> | [Jira Cloud]
                                        |                              |
           +<--(Generates Tests)------- | <--(QA AGENT: 20y Exp)---- | [Local Context]
           |   (Failing/Red State)      |    (Analyzes Specs)          |
           v                            |                              |
[ 2. Implementation ]                   |                              |
           |                            |                              |
           +--- "Make tests pass" ----> | <--(ARCHITECT AGENT)------ | [Local Context]
           |                            |    (React/TS Expert)         |
           |                            |    (Writes Min Code)         |
           |<--(Green State)----------- |                              |
           |                            |                              |
           +--- "Refactor/Polish" ----> | --(Applies Styles/Fixes)-> | [Local Context]
                                        |                              |
                                        |                              |
 PHASE 2: REVIEW & MERGE                |                              |
 (Automated Handoff)                    |                              |
                                        |                              |
[ 3. Pull Request ]                     |                              |
           |                            |                              |
           +--- "Create PR" ----------> | --(MCP: Push Branch)-----> | [GitHub Remote]
                                        | --(MCP: Create PR)-------> | [GitHub API]
                                        | --(MCP: Request Reviews)-> | [Google Chat]
                                        |                              |
                                        |                              |
 PHASE 3: QA DEPLOYMENT                 |                              |
 (Continuous Delivery)                  |                              |
                                        |                              |
[ 4. QA Release ]                       |                              |
                                        |                              |
           (PR Merged to Main) ------------------------------------> | [GitHub Actions]
                                        |                              |       |
                                        |                              |       v
                                        |                            | [GCP Cloud Build]
                                        |         (Deploy to QA) <---+-------+
                                        |                              |
                                        |                              |
 PHASE 4: PROD RELEASE                  |                              |
 (Manual Gate & Changelog)              |                              |
                                        |                              |
[ 5. Production ]                       |                              |
           |                            |                              |
           +--- "Release v1.0.0" -----> | --(MCP: Draft Release)---> | [GitHub API]
                                        |    (Gen Changelog)           |
                                        |                              |
                                        | <--(Event: Tag Pushed)---- | [GitHub Actions]
                                        |                              |       |
                                        |                              |       v
                                        |                            | [GCP Cloud Build]
                                        |       (Deploy to PROD) <---+-------+
                                        |                              |
                                        |                              |
                                        | <--(Webhook: Status)------ | [Google Chat]
                                             (Success + Changelog)
```

## ⚡ Detailed Step-by-Step

### 1️⃣ Phase 1: Inception & Context Sync
* **The Problem:** Manually finding tickets, creating branches with correct naming conventions, and moving Jira tickets takes time.
* **The Solution:**
    * Developer prompts: "Start ticket PROJ-101".
    * **Jira MCP:** Updates status to `In Progress` and assigns the ticket to the user.
    * **Git MCP:** Creates `feature/PROJ-101-ticket-title` from `main`.
    * **Context:** The Agent creates a temporary `specs.md` file in memory with the Jira description to guide code generation.

### 2️⃣ Phase 2: Agentic TDD & Implementation
* **The Prerequisite:** Detailed Jira tickets to serve as the "Source of Truth".
* **The Strategy:** Role-Based Agent Orchestration (QA vs. Architect).

* **Step 1: The QA Specialist (Red Phase)**
    * **Persona:** Senior QA Engineer (20+ years exp) focused on TDD and Edge Cases.
    * **Action:** Analyzes Jira specs and *immediately* generates the test suite.
    * **Guardrail:** If requirements are ambiguous, the Agent **pauses and asks the Developer** for clarification instead of hallucinating logic or assuming business rules.
    * **Outcome:** A comprehensive suite of failing tests (Red) that strictly validate the ticket's acceptance criteria.

* **Step 2: The React Architect (Green Phase)**
    * **Persona:** Senior Frontend Architect (20+ years exp) specialized in React, TypeScript, and Redux.
    * **Action:** Reviews the failing tests and proposes the optimal implementation strategy.
    * **Execution:** Writes the minimal, clean, and type-safe code necessary to make the tests pass (Green), ensuring correct state management patterns are applied.

* **Step 3: Refactor & Polish (Blue Phase)**
    * **Action:** Developer and Agents iterate on the solution. This includes applying **styles**, ensuring **mobile responsiveness**, accessibility compliance, and final code optimization before the commit.
### 3️⃣ Phase 3: Automated Review Request
* **The Problem:** Writing PR descriptions is tedious, and developers often forget to notify the team.
* **The Solution:**
    * Developer prompts: "Create PR".
    * **Agent:** Generates a PR description summarizing the code changes against the Jira requirements.
    * **Google Chat MCP:** Sends a formatted card to the team's space with the PR link, asking for reviews.

### 4️⃣ Phase 4: QA Deployment (The "Main" Branch)
* **Strategy:** We treat `main` as the source of truth for the QA/Staging environment.
* **Automation:**
    * When the PR receives the required approvals (e.g., 2) and is merged:
    * **GitHub Actions** triggers a Cloud Build pipeline.
    * **Cloud Build** creates the container and updates the **QA Cloud Run Service**.

### 5️⃣ Phase 5: Production Release (The Tag)
* **Strategy:** Production is protected. It does not change with every merge. It changes only with a specific **Version Tag**.
* **Automation:**
    * **Trigger:** Developer/Lead prompts: *"Create Release v1.2.0"*.
    * **Agent Action:** Calls GitHub API to draft the Release/Tag. It automatically compiles the **Changelog** based on the merged PR titles since the last release to populate the release notes.
    * **CI/CD Pipeline:** GitHub Actions detects the tag (`v*`) and triggers Google Cloud Build to promote the exact artifact from QA to Production.
* **Notification (The Feedback Loop):**
    * **Google Chat Webhook:** Upon completion, the pipeline sends a final status report to the general channel:
        * **✅ Success:** "🚀 Release **v1.2.0** deployed successfully! \n\n **Changelog:** \n - Feature: User Profile \n - Fix: Login Timeout"
        * **❌ Failure:** "🚨 Release **v1.2.0** failed during deployment. Check Cloud Build logs immediately."