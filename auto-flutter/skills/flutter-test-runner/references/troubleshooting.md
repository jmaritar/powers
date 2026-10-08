# Troubleshooting: preflight y corridas

Guia de diagnostico para la skill `flutter-test-runner`. Mapea cada problema tipico a su
causa y recomendacion.

## Chequeos del preflight (antes de correr)

| Chequeo en FALTA | Causa probable | Recomendacion |
|---|---|---|
| **Poetry** | poetry no esta en PATH | Instalar Poetry; `poetry install` en la raiz del repo. |
| **.env** | No existe `.env` | Copiar `.env.example` a `.env` y configurarlo. |
| **Ambiente** | `EXECUTION_ENVIRONMENT` vacio/raro | Poner `EXECUTION_ENVIRONMENT='staging'` (u otro ambiente valido). |
| **adb / device** (adb no responde) | platform-tools no instalado / no en PATH | Instalar Android platform-tools y agregar `adb` al PATH. |
| **adb / device** (sin dispositivos) | Emulador apagado / cable | Arrancar emulador o conectar device; `adb devices`. |
| **adb / device** (unauthorized) | Falta autorizar depuracion USB | Aceptar el dialogo en el device; re-enchufar si hace falta. |
| **adb / device** (DEVICE_NAME no coincide) | `DEVICE_NAME` del `.env` != serial real | Ajustar `DEVICE_NAME` al serial que muestra `adb devices`. |
| **Appium server** | Appium no esta corriendo | Abrir otra terminal y ejecutar `appium` (dejar corriendo). |
| **APK (TEST_MODE=apk)** | No hay `.apk` en `resources/apk/` | Colocar el `.apk`, o cambiar a `TEST_MODE=running_app`. |
| **App (running_app)** | La app no esta instalada/abierta | Instalar/abrir la app (`flutter run`) o corregir `APP_PACKAGE`. |
| **Modulo de tests** | Carpeta inexistente / mal nombre | Usar el nombre exacto de la carpeta (ojo guion vs guion_bajo: tests de ultima milla = `ultima-milla`). |

## Fallos durante la corrida (pytest)

| Sintoma | Causa probable | Recomendacion |
|---|---|---|
| `NoSuchElementException` / locator no encontrado | content-desc/hint cambio, o la pantalla no es la esperada | Revisar el screenshot `FAIL_*.png`; validar el locator en `screenplay/ui/<modulo>_*.py` contra el arbol real. |
| `TimeoutException` esperando visibilidad | Elemento tarda mas que el timeout, o build sin los Semantics | Subir el timeout del caso, o confirmar que el build bajo prueba tenga los `Semantics` requeridos. |
| Falla en el login / sesion | Credenciales de `test_data.py` (`LoginTestData`) o `.env` | Verificar usuario/clave y el ambiente; algunos tests reusan sesion entre tests (orden importa). |
| `pytest.exit` "Appium server no disponible" | Appium se cayo a mitad | Reiniciar `appium`; re-correr el preflight. |
| `pytest.exit` device no listo | Device se desconecto / paso a offline | `adb devices`; reconectar/reiniciar emulador. |
| Muchos tests en `SKIPPED` | Casos con `pytest.skip` (TODOs sin implementar) | Es esperado en modulos a medio implementar (ej. `test_incremento_um_screenplay.py`); implementar el caso o correr solo los tests listos. |
| App se cierra al terminar | `no_reset=True` + `adb force-stop` en el teardown (normal) | No es un fallo: el conftest detiene la app al final a proposito. |

## Donde mirar la evidencia

- Screenshots de fallo: `artifacts/logs/<modulo>/screenshots/FAIL_*.png`
- Videos por test: `artifacts/videos/<modulo>/<KEY>_<timestamp>.mp4`
- Reporte HTML: `artifacts/reports/html/<modulo>/report.html`
- Resultados JSON (para Xray): `artifacts/reports/json/<modulo>/results_*.json`

## Modos graficos

- **Dashboard** (`scripts\run_suite.bat <modulo> dashboard`): tablero en vivo con tests por
  estado (EN CURSO / FALLARON / PASARON / PENDIENTES), checklist de pasos del test actual y
  log en vivo. Bueno para ver una corrida completa.
- **Step-debugger** (`scripts\run_suite.bat <modulo> debug`): primero eliges qué tests correr
  en una pantalla de seleccion, luego pausa al terminar cada paso. Bueno para depurar un caso
  puntual. No conviven a la vez con el dashboard.
