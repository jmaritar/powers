---
name: flutter-test-runner
description: >-
  Levanta y ejecuta las pruebas Appium/ScreenPlay del proyecto automation-flutter de forma
  guiada: primero corre un preflight de chequeos previos (adb/device, Appium, poetry, .env,
  APK o app corriendo, modulo de tests) con recomendaciones, y solo cuando el entorno esta
  listo ejecuta el modulo elegido, opcionalmente con dashboard en vivo o step-debugger
  grafico. Usar cuando el usuario quiera correr/levantar/ejecutar pruebas, saber que pasos
  faltan antes de correr, o entender por que fallo una corrida. Palabras gatillo - correr
  pruebas, levantar tests, ejecutar modulo, preflight, chequeos previos, dashboard, step
  debugger, ultima milla, tienda, staging.
metadata:
  author: jorge.arita
  version: 1.0.0
---

# Levantar y ejecutar pruebas (Appium + ScreenPlay)

Guias al usuario, paso a paso, para levantar las pruebas del proyecto `automation-flutter`
sin fricción: primero **validas el entorno** (preflight), le dices **qué falta** y **cómo
resolverlo**, y solo cuando está listo **ejecutas** el módulo pedido. Trabaja en español.

## Herramientas del proyecto (ya existen en `scripts/`)

- `scripts/preflight.py` — chequeos previos con salida grafica (rich): poetry, `.env`,
  ambiente, adb/device, Appium server, APK o app instalada (segun `TEST_MODE`), y que exista
  la carpeta del modulo. Da recomendaciones por cada problema. Soporta `--module <m>` y
  `--json`. Sale con codigo 1 si hay bloqueantes.
- `scripts/run_suite.bat [modulo] [modo]` — launcher: corre el preflight y, **solo si pasa**,
  ejecuta el modulo. `modo` = `normal` | `dashboard` | `debug`.
- Modos graficos ya existentes que orquesta el launcher:
  - `dashboard` -> pytest con `DASHBOARD=1` (tablero TUI en vivo, `screenplay/utils/general/dashboard.py`).
  - `debug` -> step-debugger interactivo (`scripts/debug_pytest.py`, `STEP_DEBUG=1`): elige
    tests en una pantalla y pausa por paso.

Contexto de apoyo del proyecto: `#ejecucion`, `#project-structure`, `#test-data-config`.

## Flujo recomendado (conversacional)

### Paso 1 — Confirmar qué correr
Pregunta (si no lo dio): **módulo** (carpeta bajo `screenplay/tests/`, ej. `ultima-milla`,
`tienda`, `login`) y **modo** (`normal`, `dashboard`, `debug`). Recuerda el detalle de nombres:
la carpeta de tests de última milla es `ultima-milla` (con guion), aunque las tasks estén en
`ultima_milla` (guion bajo).

### Paso 2 — Correr el preflight
Indica ejecutar (o hazlo tú vía terminal, es no destructivo):
```
poetry run python scripts/preflight.py --module <modulo>
```
Lee el resultado. Si hay chequeos en **FALTA**, NO sigas a la ejecución: resume qué falta y
las recomendaciones (ver `references/troubleshooting.md`). Los pasos previos tipicos que el
usuario debe cubrir a mano:
- **Appium**: abrir otra terminal y ejecutar `appium` (proceso de larga duración; el usuario
  lo corre, tú no lo lances).
- **Device**: `adb devices` debe listar el `DEVICE_NAME` del `.env` como `device`.
- **App/APK**: en `TEST_MODE=apk`, un `.apk` en `resources/apk/`; en `running_app`, la app
  abierta e instalada (`APP_PACKAGE`).
- **Ambiente**: para staging, `EXECUTION_ENVIRONMENT='staging'` en `.env`.

### Paso 3 — Ejecutar (solo si el preflight quedó LISTO)
Usa el launcher, que vuelve a correr el preflight como puerta de entrada:
```
scripts\run_suite.bat <modulo> <modo>
```
Ejemplos:
```
scripts\run_suite.bat ultima-milla            REM normal + reporte HTML
scripts\run_suite.bat ultima-milla dashboard  REM tablero en vivo
scripts\run_suite.bat ultima-milla debug       REM step-debugger grafico
```
Alternativa directa sin launcher (si el usuario prefiere): fija `PYTEST_MODULE_NAME` y corre
`poetry run pytest screenplay/tests/<modulo>/ -v -s`.

### Paso 4 — Interpretar el resultado
- Si pasó: indica dónde quedaron los artefactos (`artifacts/videos/<modulo>/`,
  `artifacts/reports/html/<modulo>/`, `.../json/<modulo>/`).
- Si falló: revisa el screenshot del fallo en `artifacts/logs/<modulo>/screenshots/FAIL_*.png`
  y el video, y da recomendaciones segun `references/troubleshooting.md` (locator no
  encontrado, timeout, sesión/login, device desconectado, etc.).

## No hagas (guardrails)

- No lances Appium, emuladores, `flutter run` ni el propio pytest como procesos de larga
  duración desde el agente; indica el comando para que el usuario lo corra en su terminal.
- No saltes el preflight salvo que el usuario lo pida explícitamente (`SKIP_PREFLIGHT=1`).
- No inventes datos, keys de Xray ni content-desc. No imprimas secretos del `.env`.
- Si el `.env` tiene tokens reales expuestos, adviértelo y sugiere rotarlos; no los muestres.
- Trabaja en español y un paso a la vez en los puntos de decisión.
