---
name: "auto-flutter"
displayName: "Auto Flutter"
description: "Asistente para escribir pruebas automatizadas Appium sobre apps Flutter/Android (POS RedPos y Service) con el patron ScreenPlay: locators por content-desc/hint, Interactions, Tasks, Questions, test_data y markers de Xray. Basado en el proyecto automation-flutter."
keywords: ["flutter", "appium", "automatizacion", "pruebas", "test", "screenplay", "pos", "redpos", "xray", "pytest", "locators", "pdc", "e2e", "movil"]
author: "Vikingo IA"
---

# Cuando usar este power

Cuando trabajes en la suite de automatizacion **Appium + ScreenPlay** del proyecto
`automation-flutter` (apps POS "RedPos" y Service en Flutter/Android): escribir una prueba
nueva de punta a punta, agregar locators de una pantalla, crear una Interaction/Task/Question,
mapear datos en `test_data`, marcar un test con su key de Xray, o entender como esta armada
la suite para modificarla con seguridad.

Palabras gatillo: "escribir un test", "automatizar un caso", "nueva prueba Appium",
"agregar un locator", "crear una task de screenplay", "test de tienda/deposito/login", etc.

# Skills incluidas

- `flutter-test-author` (`/flutter-test-author`) — conduce, paso a paso, la escritura de una
  prueba automatizada nueva siguiendo el patron ScreenPlay del proyecto: ubica el modulo,
  define/reutiliza locators, arma las Tasks y Questions, cablea los datos de `test_data`,
  escribe el test con sus markers de Xray y explica como ejecutarlo.
- `flutter-test-runner` (`/flutter-test-runner`) — levanta y ejecuta las pruebas de forma
  guiada: corre un preflight de chequeos previos (adb/device, Appium, poetry, `.env`, APK o
  app corriendo, modulo) con recomendaciones, indica que pasos faltan, y solo cuando el
  entorno esta listo ejecuta el modulo (modo normal, dashboard en vivo o step-debugger
  grafico). Ante fallos, orienta con troubleshooting.

# Cuando cargar cada steering

Ante una tarea relacionada, carga el steering correspondiente de `./steering/`:

- Entender el patron ScreenPlay (Actor/Abilities/Tasks/Interactions/Questions/Matchers)
  y sus contratos -> `./steering/00-screenplay-pattern.md`
- Escribir locators para una app Flutter (content-desc / hint, listas de fallback,
  locators dinamicos) -> `./steering/01-locators-flutter.md`
- Estructura del proyecto, modulos, fixtures del conftest y markers de Xray
  -> `./steering/02-project-structure.md`
- Datos de prueba: ConfigurationManager, `resources/test_data/*.json` y sus getters
  -> `./steering/03-test-data-config.md`
- Como correr las pruebas (entorno, Appium, emulador, scripts .bat, modos)
  -> `./steering/04-ejecucion.md`

# Scripts en el proyecto (los usa la skill `flutter-test-runner`)

Viven en el repo `automation-flutter/scripts/` (no en este power), y la skill los orquesta:

- `preflight.py` — chequeos previos graficos (rich) + recomendaciones:
  `poetry run python scripts/preflight.py --module <modulo>`.
- `run_suite.bat <modulo> [normal|dashboard|debug]` — corre el preflight y, si pasa, ejecuta
  el modulo en el modo elegido (dashboard = tablero en vivo; debug = step-debugger grafico).

# Requisitos previos (en la maquina donde se ejecutan las pruebas)

Este power solo aporta guia y convenciones; NO reemplaza el entorno de ejecucion. Para
correr las pruebas del proyecto `automation-flutter` hace falta:

1. Python 3.11+ y Poetry (`poetry install`).
2. Un emulador/dispositivo Android visible en `adb devices`.
3. Appium server corriendo (por defecto `http://127.0.0.1:4723`).
4. La app (POS/Service) abierta si se usa `TEST_MODE=running_app`, o un `.apk` en
   `resources/apk/` si se usa `TEST_MODE=apk`.
5. `.env` configurado a partir de `.env.example`.

# Seguridad

- No contiene tokens ni credenciales. Los secretos de Xray/GitLab van en `.env` (fuera de
  este power) o en la config MCP del usuario.
- Los ejemplos de datos (usuarios, NIT, montos) son de referencia; los reales viven en
  `resources/test_data/*.json` del proyecto, no aqui.

# Licencia

MIT. Ver el `LICENSE` en la raiz del repositorio.
