---
name: app-context
description: >-
  Deja una sesion lista para trabajar un cambio en la APP Flutter (ecosistema-vikingo-app)
  sin orquestador - posiciona el proyecto y carga lo esencial: UM=tms, carpetas de la app y
  estandares Flutter. Usar para un fix o cambio directo en la app. Palabras gatillo -
  cambio en la app, trabajar en flutter, app-context, fix app, itinerario, deposit_page.
metadata:
  author: jorge.arita
  version: 1.0.0
---

# Skill: App Context (sesion Flutter lista de un tiro)

Deja la sesion posicionada y con lo ESENCIAL cargado para un cambio directo en la APP,
sin orquestador y sin el flujo completo de spec. Carga solo lo que la app necesita.

## Pasos

1. **Posiciona el proyecto:** `set_project` a `C:\PDC\frontend\ecosistema-vikingo-app`.
2. **Carga negocio minimo (UM=tms):** lee `C:\PDC\intelligence\knowledge\design-system\flutter.md`
   y el steering `01-um-codigo.md` del power `um-domain` (mapa UM=tms + las 2 carpetas de la app).
3. **Confirma el scope de la app (SON DOS carpetas):**
   - `lib/pages/ffa/itinerary`  (operacion del piloto; UM = rutas de Entregas)
   - `lib/pages/tms/deposit_page` (depositos)
4. **Carga estandares Flutter:** tokens de `lib/values/` (CustomColors, CustomTypograpy,
   CustomDimensions) + reglas de `design-system/flutter.md` + lessons Flutter
   (Clean Arch feature-first, Riverpod, nunca hardcodear).
5. **Reporta en una linea:** proyecto activo + que es un cambio tipo fix + listo para el cambio.

## Lo que NO se carga (para no quemar tokens)
WEB/Angular, repos de pruebas, backend, flujo PRD->Spec completo, orquestador/STATE.md.

## Reglas del cambio (camino fix)
- Es un fix/cambio sin PRD: abrir issue JIRA (Task/Bug) para trazabilidad.
- Scope control: trabajar SOLO en la carpeta de la feature; salir de ahi requiere confirmacion.
- Estandares SIEMPRE: consumir tokens, nunca hardcodear color/fuente/espaciado/radio/sombra.
- Si el cambio toca un flujo probado, dejar evidencia (video) al cerrar.

## Dato critico
UM = `tms` en el codigo. Buscar `tms`, no `ultima-milla`.
