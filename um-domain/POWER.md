---
name: "um-domain"
displayName: "UM Domain"
description: >-
  Conocimiento de negocio de Ultima Milla (UM) del ecosistema Vikingo: tipos de ruta
  (crossdock/padre/hija), flujos GLT y AVON, actividades, cancelaciones y el mapa clave
  UM=tms en el codigo. Steering puro, sin skills. Lo carga cualquier sesion (WEB, APP o
  pruebas) que necesite el contexto de negocio de UM.
keywords: ["ultima milla", "um", "tms", "crossdock", "itinerario", "entregas", "glt", "avon", "ruta hija", "liquidacion", "vikingo"]
author: "Vikingo IA"
---

# Cuando usar este power

Cuando trabajes cualquier cosa de Ultima Milla y necesites el contexto de NEGOCIO:
que es una ruta crossdock/padre/hija, el flujo GLT vs AVON, las actividades por tipo
de ruta, los 4 niveles de cancelacion, el cierre administrativo, o el dato critico de
que UM = `tms` en el codigo. Es el "cerebro de negocio" compartido entre WEB, APP y pruebas.

Palabras gatillo: "ultima milla", "um", "tms", "crossdock", "ruta hija", "glt", "avon",
"itinerario", "entregas", "liquidacion de ruta".

# Skills incluidas

- `app-context` (`/app-context`) — deja una sesion lista para un cambio directo en la APP
  Flutter sin orquestador: posiciona el proyecto `ecosistema-vikingo-app` y carga lo esencial
  (UM=tms, las 2 carpetas de la app, estandares Flutter). Para fixes/cambios sin PRD.

# Cuando cargar cada steering

- Modelo de negocio UM (rutas, actividades, GLT/AVON, cancelaciones) -> `./steering/00-um-negocio.md`
- Mapa de codigo UM=tms y donde vive cada parte -> `./steering/01-um-codigo.md`

# Base de conocimiento asociada

El conocimiento vivo y versionable esta en `C:\PDC\intelligence\knowledge\` (INDEX.md,
um/*, features/, worklog/). Este power es la capa CURADA; esa carpeta es la capa VIVA.

# Seguridad

No contiene credenciales. Rutas de codigo son de referencia local del equipo PDC.

# Licencia

MIT. Ver el `LICENSE` en la raiz del repositorio.
