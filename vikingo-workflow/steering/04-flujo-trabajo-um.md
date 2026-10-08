---
inclusion: manual
name: flujo-trabajo-um
description: Los dos caminos de trabajo - flujo completo (PRD/DERCAS) y flujo fix (cambio sin requerimiento), con los pasos de cada uno.
---

# Flujo de trabajo UM — dos caminos

Todo cambio pasa por JIRA (trazabilidad), pero NO todo cambio nace de un PRD.

## Regla de decisión
¿El cambio está descrito en PRD/DERCAS (GitBook)? → **Flujo completo**.
¿Es un fix / mejora / deuda técnica sin requerimiento documentado? → **Flujo fix**.

## Flujo COMPLETO (con PRD/DERCAS)
1. Leer épica JIRA (MCP atlassian) → extraer links GitBook.
2. Leer PRD/DERCAS en GitBook (MCP gitbook) → casos de uso + criterios.
3. Spec de Kiro (requirements → design → tasks) en el repo de trabajo.
4. Visto bueno del usuario sobre tasks.md.
5. Crear issues en JIRA.
6. Desarrollo (tarea por tarea; issue a In Progress al iniciar).
7. Pruebas automatizadas según lo definido + **evidencia de video al ticket JIRA** (obligatorio).
8. Actualizar JIRA + destilar ficha en `knowledge\features\` + worklog.

## Flujo FIX (sin PRD/DERCAS)
1. Abrir issue JIRA (Task/Bug, NO Epic). Es el registro mínimo obligatorio.
2. Mini-spec OPCIONAL (solo tasks.md o una nota) — según tamaño del cambio.
3. Visto bueno si el cambio tiene impacto; para fixes triviales basta el issue.
4. Desarrollo.
5. Pruebas si aplica + evidencia de video si toca flujo probado.
6. Actualizar JIRA + ficha ligera + worklog.

## Lo que SIEMPRE se cumple (ambos caminos)
- Hay un issue de JIRA (trazabilidad).
- Nada se crea/transiciona en JIRA sin visto bueno (salvo el issue del fix que tú autorizas).
- El trabajo queda en el worklog (ayer/hoy) y en `STATE.md` por fase.
- Scope control: cada sesión en su carpeta de feature.

## MCP que se usan (carga perezosa)
- `atlassian` (JIRA), `gitbook` (PRD/DERCAS), `gitlab` (commits/MR). Solo cuando el paso lo pide.
- Teams: envío deshabilitado (sin permisos) → el worklog se recolecta en sesión, no en Teams.
