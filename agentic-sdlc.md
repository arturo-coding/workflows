# Agentic SDLC: Context-Driven Delivery (QA & Prod)

**Architecture:** Model Context Protocol (MCP)
**Strategy:** GitFlow Modified (`main` is QA, `tags` are Production)

## 🔄 The Master Workflow

This diagram represents the entire lifecycle from a Jira Ticket to a Production Release. The **Intelligent Editor** acts as the central orchestrator, bridging the gap between Project Management (Jira) and Infrastructure (GitHub/GCP) without the developer leaving the code interface.

```text
                               PHASE 1: INCEPTION & CODING
                               ---------------------------
      [ JIRA CLOUD ]                   [ EDITOR / AGENT ]                  [ GITHUB REPO ]
            |                                  |                                  |
    (1) Status: TO DO                          |                                  |
            | <----(MCP: Read Ticket)--------- |                                  |
            |                                  |                                  |
    (2) Status: IN PROGRESS                    |                                  |
            | <----(MCP: Update Status)------- |                                  |
            |                                  |                                  |
            |                                  | --(MCP: Create Branch)---------> | (3) Branch Created
            |                                  |                                  |     From: main (QA)
            |                                  |                                  |     Name: feature/TICKET-ID
                                               |
                               PHASE 2: REVIEW & MERGE (QA)
                               ----------------------------
            |                                  |                                  |
            |                                  | --(MCP: Create PR)-------------> | (4) Pull Request
            |                                  |                                  |     Base: main
            | <----(MCP: Comment Link)-------- |                                  |
            |                                  |                                  |
            |                                  [ HUMAN APPROVAL ]                 |
            |                                          |                          |
            |                                          v                          |
            |                                  (GitHub Action) -----------------> | (5) MERGE to MAIN
            |                                                                     |
            |                                                                     | (6) Webhook Trigger
            |                                                                     v
            |                                                              [ GIMP CLOUD BUILD ]
            |                                                                     |
            |                                                                     v
            |                                                             (7) DEPLOY TO QA
                                                                         (Cloud Run: Staging)

                               PHASE 3: PRODUCTION RELEASE
                               ---------------------------
            |                                  |                                  |
    (8) Status: DONE                           |                                  |
            | <----(MCP: Close Ticket)-------- |                                  |
            |                                  |                                  |
            |                                  | --(MCP: Create Release)--------> | (9) CREATE TAG (v1.x.x)
            |                                  |                                  |
            |                                                                     | (10) Webhook Trigger
            |                                                                     v
            |                                                              [ GIMP CLOUD BUILD ]
            |                                                                     |
            |                                                                     v
            |                                                             (11) DEPLOY TO PROD
                                                                           (Cloud Run: Prod)
