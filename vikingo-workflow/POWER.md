---
name: "vikingo-workflow"
displayName: "Vikingo Workflow"
description: "Flujo de trabajo de PDC en español: PRD/DERCAS (GitBook) -> Spec de Kiro -> issues en JIRA. Incluye la skill de inicializacion de features desde una epica y un indice de comandos."
keywords: ["pdc", "vikingo", "feature", "epica", "jira", "gitbook", "prd", "dercas", "casos de uso", "spec", "estimaciones", "comandos", "ayuda"]
author: "Vikingo IA"
---

# Cuando usar este power

Cuando trabajes una feature de PDC siguiendo el flujo PRD/DERCAS -> Spec -> JIRA: iniciar
el contexto de una feature desde una epica de JIRA, ubicar y leer los casos de uso (CU) del
PRD/DERCAS en GitBook, generar el Spec (requirements/design/tasks), estimar con story points
Fibonacci y crear/actualizar issues en JIRA. Tambien cuando el usuario pida "que comandos hay"
o ayuda sobre las skills disponibles.

# Skills incluidas

- `comandos` (`/comandos`) — indice directo, en español, de todos los comandos disponibles
  (skills `/`, steering `#`, plantillas y flujos).
- `feature-workspace-init` (`/feature-workspace-init`) — inicializa el contexto de una feature
  desde una epica de JIRA de forma conversacional (lee epica, ubica PRD/DERCAS, lista CU,
  valida issues existentes y ofrece crear los faltantes, con confirmacion y rollback).
- `orchestrator-resume` (`/orchestrator-resume`) — levanta un chat nuevo como Orquestador UM
  leyendo el estado durable de `C:\PDC\intelligence\knowledge\orchestrator\` (chat desechable).

# Cuando cargar cada steering

Ante una tarea relacionada, carga el steering correspondiente de `./steering/`:

- Entender el pipeline completo PRD/DERCAS -> Spec -> JIRA -> `./steering/00-workflow-prd-to-jira.md`
- Los dos caminos de trabajo (flujo completo vs flujo fix) -> `./steering/04-flujo-trabajo-um.md`
- Estimar tareas con story points Fibonacci -> `./steering/01-estimations.md`
- Defaults de JIRA (sitio, cloudId, project key, tipos, customfields) -> `./steering/02-atlassian-defaults.md`
- Ubicar PRD/DERCAS en GitBook (organizacion y spaces) -> `./steering/03-gitbook-context.md`
- Molde de un Spec (requirements/design/tasks) -> `./steering/_spec-template.md`
- Indice central de features trabajadas -> `./steering/features/INDEX.md`

# Configuracion previa (una vez por dispositivo)

1. **MCP** en `~/.kiro/settings/mcp.json`: servidores `atlassian` y `gitbook` (OAuth o token).
   Los secretos NO viven en este power.
2. **Rellenar placeholders** `<< ... >>` del steering con tus valores reales
   (`02-atlassian-defaults.md`, `03-gitbook-context.md`).

# Seguridad

- No contiene tokens ni credenciales. Van en `~/.kiro/settings/mcp.json`, fuera del repo.
- cloudId, project key y customfields vienen como placeholders para que cada equipo ponga los suyos.
- No incluye fichas de features en curso ni rollback-logs (contexto interno).

# Licencia

MIT. Ver el `LICENSE` en la raiz del repositorio.
