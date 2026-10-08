---
inclusion: manual
name: ejecucion
description: Como ejecutar las pruebas de automation-flutter - requisitos (Appium, emulador, .env), modos running_app vs apk, scripts .bat por modulo y comandos pytest directos.
---

# Ejecucion de pruebas (automation-flutter)

## Requisitos previos

1. **Python 3.11+ y Poetry**: `poetry install` (la primera vez, `poetry lock` si avisa).
2. **Emulador/dispositivo Android** visible: `adb devices` debe listarlo como `device`
   (no `unauthorized`/`offline`).
3. **Appium server** corriendo (default `http://127.0.0.1:4723`). El fixture de sesion
   aborta si no responde `/status`.
4. **App**: en modo `running_app` la app POS/Service debe estar **abierta** (flutter run o
   ya instalada). En modo `apk` debe haber un `.apk` en `resources/apk/`.
5. **.env** configurado (copiar de `.env.example`).

## Variables de entorno clave (.env)

```
APPIUM_SERVER=http://127.0.0.1:4723
DEVICE_NAME=emulator-5554
PLATFORM_VERSION=13
TEST_MODE=running_app        # running_app (dev) | apk (CI)
APP_PACKAGE=com.redpos.legacy
APP_ACTIVITY=.MainActivity
PYTEST_MODULE_NAME=general   # organiza reportes/videos por modulo
TEST_ENV=default             # elige resources/test_data/<TEST_ENV>.json
```

## Scripts .bat (recomendado, Windows)

```bat
scripts\run_tests.bat tienda        REM corre screenplay/tests/tienda/
scripts\run_tests.bat login
scripts\run_tests.bat deposito
scripts\run_tests.bat general       REM corre todos los tests
scripts\run_specific_test.bat POS-123   REM por key de Xray (-k)
```

`run_tests.bat` fija `PYTEST_MODULE_NAME`, limpia videos previos del modulo, genera reporte
HTML en `artifacts/reports/html/<modulo>/` y videos en `artifacts/videos/<modulo>/`.

## pytest directo

```bash
poetry run pytest screenplay/tests/tienda/ -v -s
poetry run pytest screenplay/tests/ -k "TTDEV-12743" -v
poetry run pytest screenplay/tests/tienda/ --html=artifacts/reports/html/tienda/report.html --self-contained-html
```

## Salidas (artifacts/)

- `videos/<modulo>/<KEY>_<timestamp>.mp4` — grabacion por test.
- `reports/html/<modulo>/report.html` — reporte HTML.
- `reports/json/<modulo>/results_*.json` — resultados (para subir a Xray).
- `logs/<modulo>/screenshots/FAIL_*.png` — screenshot en fallos.

## Xray (opcional)

Con `XRAY_CLIENT_ID/SECRET` y demas vars en `.env`, subir resultados con
`scripts\upload_to_xray.bat` (o `scripts/upload_results_to_xray.py`).

## Nota para el agente

- **No** lances Appium, `flutter run`, emuladores ni el propio pytest como procesos de larga
  duracion desde el asistente: solo indica el comando para que el usuario lo corra en su
  terminal.
- Modos extra de UI para debug (opt-in por env var): `DASHBOARD=1` (dashboard TUI),
  `STEP_DEBUG=1` via `scripts/debug_pytest.py` (debugger paso a paso). No conviven a la vez.
