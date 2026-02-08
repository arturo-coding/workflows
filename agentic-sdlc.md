# Agentic SDLC: The Context-Driven Pipeline

**Concept:** This workflow replaces rigid DevOps scripts with an **Agentic Orchestrator**. By using the **Model Context Protocol (MCP)**, the Code Editor becomes the central command center, managing the entire lifecycle—from ticket retrieval to cloud deployment—without the developer ever leaving the IDE.

---

## 🔄 The Flow: Architecture & Phases

The following diagram illustrates the interaction between the Developer, the AI Agent (embedded in the Editor), and External Infrastructure Services across the four key phases of development.

```text
+-----------------+       +---------------------------+       +-------------------------+
|    DEVELOPER    |       |   AGENTIC EDITOR (MCP)    |       |  CLOUD INFRASTRUCTURE   |
| (Prompt/Action) |       | (Orchestrator + Context)  |       | (Jira / GitHub / GCP)   |
+--------+--------+       +-------------+-------------+       +------------+------------+
         |                              |                                  |
         |                              |                                  |
=========+==============================+==================================+=========
  PHASE 1: INCEPTION & CONTEXT LOADING (Jira -> Local)
=========+==============================+==================================+=========
         |                              |                                  |
   "Start|assigned task"        [CALL: jira.get_issues]                    |
         +----------------------------->|--------------------------------->| [JIRA API]
         |                              |                                  |
         |                       [READ: Ticket Specs]                      |
         |<-----------------------------|<---------------------------------| (Return: JSON)
         |                              |                                  |
 "Confirm|Task PROJ-101"         [CALL: git.create_branch]                 |
         +----------------------------->|--------------------------------->| [GITHUB API]
         |                              |                                  |
         |                       [ACTION: Checkout Local]                  |
         |<-----------------------------|                                  |
         |                              |                                  |
         |                              |                                  |
=========+==============================+==================================+=========
  PHASE 2: DEVELOPMENT LOOP (Local Context)
=========+==============================+==================================+=========
         |                              |                                  |
   [Write Code]                  [ANALYSIS: Real-time]                     |
         |----------------------------->|                                  |
         |                              |                                  |
         |                       [CHECK: vs Jira Specs]                    |
         |<-----------------------------|                                  |
   "Does |his meet reqs?"               |                                  |
         +----------------------------->|                                  |
         |                       [REPLY: "Missing Error"]                  |
         |<-----------------------------|                                  |
         |                              |                                  |
         |                              |                                  |
=========+==============================+==================================+=========
  PHASE 3: DELIVERY & REVIEW (Local -> Remote)
=========+==============================+==================================+=========
         |                              |                                  |
  "Ready.|Open PR"               [CALL: git.push_changes]                  |
         +----------------------------->|--------------------------------->| [GITHUB REMOTE]
         |                              |                                  |
         |                       [GENERATE: PR Description]                |
         |                       (Synthesize Diffs + Jira)                 |
         |                              |                                  |
         |                       [CALL: git.create_pr]                     |
         |                              |--------------------------------->| [GITHUB PR]
         |                              |                                  |
         |                       [CALL: jira.update_status]                |
         |                              |--------------------------------->| [JIRA "In Review"]
         |                              |                                  |
         |                              |                                  |
=========+==============================+==================================+=========
  PHASE 4: DEPLOYMENT (Automated)
=========+==============================+==================================+=========
         |                              |                                  |
         |                       [EVENT: PR Merged]                        |
         |                              |<---------------------------------| [GITHUB ACTION]
         |                              |                                  |
         |                       [TRIGGER: Cloud Build]                    |
         |                              |--------------------------------->| [GCP BUILD]
         |                              |                                  |
   "Is it|Live?"                 [POLL: gcp.get_status]                    |
         +----------------------------->|--------------------------------->| [CLOUD RUN]
         |                              |                                  |
         |                       [NOTIFY: "Deploy Success"]                |
         |<-----------------------------|                                  |
         |                              |                                  |
+--------+--------+       +-------------+-------------+       +------------+------------+
