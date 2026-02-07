# Automated SDLC: Jira to Google Cloud Platform

This workflow outlines a "Zero-Touch" approach to the development cycle. The goal is to reduce manual context switching by using **Jira** as the source of truth that orchestrates **GitHub** and **Google Cloud Platform**.

## 🧩 The Architecture

The system uses an Event-Driven architecture. No manual branch creation or manual deployments are required.

```mermaid
graph TD
    %% Nodes
    Jira["User (Jira)"] 
    Webhook["Webhook / Cloud Function"]
    GitHub["GitHub Repo"]
    CI["CI Pipeline (Tests/Lint)"]
    CloudBuild["Google Cloud Build"]
    GCP["GCP (Cloud Run/App Engine)"]

    %% Flow
    Jira -->|Move Ticket to 'In Progress'| Webhook
    Webhook -->|Create Branch 'feature/TICKET-ID'| GitHub
    GitHub -->|Developer Pushes Code| CI
    CI -->|Checks Passed| GitHub
    
    GitHub -->|Pull Request Merged| CloudBuild
    CloudBuild -->|Build Container & Deploy| GCP
    
    GCP -.->|Update Ticket 'Deployed'| Jira

    style Webhook fill:#f9f,stroke:#333,stroke-width:2px
    style CloudBuild fill:#4285F4,stroke:#333,stroke-width:2px,color:white
