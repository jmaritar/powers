# Vikingo Skills — repositorio de Powers de PDC

Coleccion de **Kiro Powers** de PDC, en español. Cada power aporta skills, steering y
plantillas para tareas especificas. Modelado segun el repositorio oficial
[kirodotdev/powers](https://github.com/kirodotdev/powers): un repo = varios powers, cada
uno en su propia carpeta.

Documentacion de Powers: https://kiro.dev/docs/powers/

## Powers disponibles

### vikingo-workflow
**Vikingo Workflow** - Flujo de trabajo de PDC en español: PRD/DERCAS (GitBook) -> Spec de
Kiro -> issues en JIRA. Incluye la skill de inicializacion de features desde una epica
(`/feature-workspace-init`) y un indice de comandos (`/comandos`). Trae steering con defaults
de Atlassian, contexto de GitBook, estimaciones (story points Fibonacci) y plantillas de Spec.

**Skills:** comandos, feature-workspace-init
**MCP Servers:** ninguno incluido (usa tus servidores `atlassian` y `gitbook` de `~/.kiro/settings/mcp.json`)

### auto-flutter
**Auto Flutter** - Asistente para escribir pruebas automatizadas Appium sobre apps
Flutter/Android (POS RedPos y Service) con el patron ScreenPlay del proyecto
`automation-flutter`. Guia paso a paso la escritura de una prueba end-to-end
(`/flutter-test-author`): locators por content-desc/hint, Interactions, Tasks, Questions,
datos de `test_data` y markers de Xray. Trae steering con el patron ScreenPlay, locators
Flutter, estructura del proyecto, ConfigurationManager y ejecucion.

Tambien trae la skill `/flutter-test-runner`, que levanta las pruebas de forma guiada: corre
un preflight de chequeos previos (adb/device, Appium, poetry, `.env`, APK o app corriendo,
modulo) con recomendaciones e indica que pasos faltan, y solo cuando el entorno esta listo
ejecuta el modulo (normal, dashboard en vivo o step-debugger grafico), con troubleshooting
ante fallos. Se apoya en `automation-flutter/scripts/preflight.py` y `run_suite.bat`.

**Skills:** flutter-test-author, flutter-test-runner
**MCP Servers:** ninguno (usa el entorno del proyecto: Poetry, Appium, emulador, `.env`)

### um-domain
**UM Domain** - Conocimiento de NEGOCIO de Ultima Milla (crossdock/padre/hija, GLT/AVON,
cancelaciones, mapa UM=tms y donde vive cada parte). Steering puro, sin skills: es el
"cerebro de negocio" liviano que cargan WEB, APP y pruebas. La base de conocimiento viva
(fichas, worklog, estado del orquestador) vive aparte en `C:\PDC\intelligence\knowledge\`.

**Skills:** app-context (deja una sesion Flutter lista para un fix sin orquestador)
**MCP Servers:** ninguno

---

> Proximos powers (roadmap): `vikingo-angular`, `vikingo-backend`.

## Estructura del repositorio

```
powers/                          # repo = marketplace de powers
├── README.md                    # este indice de powers
├── CONTRIBUTING.md              # como agregar un power / una skill
├── LICENSE
├── vikingo-workflow/            # un power = una carpeta
│   ├── POWER.md                 # manifiesto (displayName, author, keywords, steering)
│   ├── assets/                  # logo del power (casco Vikingo)
│   ├── skills/
│   │   ├── comandos/
│   │   └── feature-workspace-init/
│   └── steering/                # steering (#...) + plantillas
└── auto-flutter/                # power de automatizacion Appium/ScreenPlay
    ├── POWER.md
    ├── skills/
    │   └── flutter-test-author/ # SKILL.md + references/
    └── steering/                # screenplay-pattern, locators-flutter, ...
```

Cada power usa el formato **`POWER.md`** (soporta `displayName` y `author`, que es lo que el
IDE muestra como nombre y "by"). Ver `CONTRIBUTING.md` para agregar un power nuevo.

## Instalacion (Kiro IDE)

Panel de **Powers** → **Add Custom Power** → **Import power from GitHub**, y apunta a la
**carpeta del power** dentro del repo (no a la raiz). Por ejemplo:

```
https://github.com/jmaritar/powers/tree/main/vikingo-workflow
```

O bien **Import power from a folder** y selecciona `vikingo-workflow/` para probar en local.

La activacion es dinamica por `keywords`: Kiro carga el power cuando tu tarea coincide.

## Nota sobre el logo en el IDE

El casco Vikingo (`vikingo-workflow/assets/logo.svg`) se ve en GitHub, pero **el IDE aun no
muestra logo para powers personalizados** — es una funcionalidad pendiente de Kiro
(issue [kirodotdev/powers#103](https://github.com/kirodotdev/powers/issues/103)). El nombre
"Vikingo Workflow" y el "by Vikingo IA" sí se muestran (vienen de `displayName` y `author`
en `POWER.md`). Para branding con logo, el power debe entrar al registro oficial:
https://kiro.dev/powers/submit

## Licencia

MIT. Ver `LICENSE`.
