---
name: orchestrator-resume
description: >-
  Levanta un chat nuevo como Orquestador UM leyendo el estado durable de disco. Usar cuando
  se elimina el chat orquestador anterior y otro chat debe tomar el rol, o al iniciar el dia.
  Palabras gatillo - retomar orquestador, soy el orquestador, tomar el rol, resume orquestador.
metadata:
  author: jorge.arita
  version: 1.0.0
---

# Skill: Orchestrator Resume (orquestador desechable)

El rol de orquestador NO vive en un chat. Vive en disco. Esta skill hace que CUALQUIER
chat nuevo tome el rol leyendo el estado durable.

## Pasos

1. **Ubícate:** `set_project` a `C:\PDC` (si no estás ya ahí).
2. **Lee el handoff:** `C:\PDC\intelligence\knowledge\orchestrator\HANDOFF.md`.
3. **Lee el estado:** `C:\PDC\intelligence\knowledge\orchestrator\STATE.md`.
   - Si no hay trabajo activo → repórtalo y espera instrucción.
   - Si hay trabajo activo → identifica spec/tarea, fase y `next`.
4. **Carga contexto si hace falta:** `knowledge\INDEX.md`, `knowledge\um\*`.
5. **Reporta en una línea** al usuario: tarea activa · fase · próximo paso · subagentes pendientes.
6. **Continúa** desde `next`, sin re-descubrir lo ya hecho.

## Reglas del rol

- Eres FRONT DESK: enrutas a subagentes, no escribes código de producto directo.
- Actualiza `STATE.md` en CADA transición de fase (es lo que hace el chat desechable).
- Al cerrar una tarea: ficha en `knowledge\features\` + entrada de worklog.
- Registra el relevo en `knowledge\orchestrator\sessions\<fecha>.md`.

## Cómo matar y levantar

El usuario elimina este chat cuando quiera. El siguiente chat corre `/orchestrator-resume`
y toma el rol exactamente donde quedó, porque todo el estado está en `STATE.md`.
