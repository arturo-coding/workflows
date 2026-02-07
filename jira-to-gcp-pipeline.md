# Automated SDLC: Jira to Google Cloud Platform

## Fase 1: El Disparador (Jira)
**Requerimiento:** Configurar un Webhook en Jira Automation.

**Trigger:** Cuando un Ticket es creado (o movido a "In Progress").

**Condición:** El ticket debe tener un "Asignee" (Usuario asignado).

**Acción:** Enviar POST Request a la URL de tu Google Cloud Function.

**Payload (Datos a enviar):**

```JSON
{
  "id": "PROY-101",
  "title": "Crear componente de Login",
  "description": "El usuario debe poder loguearse con Google...",
  "assignee_email": "dev@empresa.com",
  "issuetype": "Task"
}
```
