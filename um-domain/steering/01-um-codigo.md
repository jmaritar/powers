---
inclusion: manual
name: um-codigo
description: Mapa de codigo de Ultima Milla - UM=tms y donde vive cada parte (WEB, APP, pruebas, backend).
---

# UM — Mapa de código

## DATO CRÍTICO
**UM = `tms` en el código.** Buscar `ultima-milla` no encuentra nada. Buscar `tms`,
`tms.service.ts`, `tmsUrl`, SPs `TMS_*`, markers `tms`.

## Dónde vive cada parte
| Parte | Stack | Ruta |
|-------|-------|------|
| WEB | Angular | `C:\PDC\frontend\administrador-vikingo\src\app\pages\tms` |
| APP itinerario | Flutter | `C:\PDC\frontend\ecosistema-vikingo-app\lib\pages\ffa\itinerary` |
| APP depósitos | Flutter | `C:\PDC\frontend\ecosistema-vikingo-app\lib\pages\tms\deposit_page` |
| Pruebas APP | Flutter/Screenplay | `C:\PDC\frontend\automation-flutter\screenplay\tests\ultima-milla` |
| Pruebas WEB | Angular/Playwright | `C:\PDC\frontend\automation-angular\src\tests\ultima-milla` |
| Backend | APIs + SPs | `C:\PDC\backend\ffa-api` · `C:\PDC\backend\tms-service` |

## Notas de scope
- En la APP, UM vive en DOS carpetas (itinerario + deposit_page).
- Dependencias WEB fuera de `tms`: `ffa/route`, `ffa/activitys`, `ffa/users/agents`,
  `vikingo/reference-user`.
- Cada sesión trabaja SOLO en su carpeta de feature; salir de ahí requiere confirmación.
