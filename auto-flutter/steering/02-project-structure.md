---
inclusion: manual
name: project-structure
description: Estructura del proyecto automation-flutter - carpetas de screenplay, modulos de tests, fixtures del conftest (driver, video_recorder, test_config) y markers de Xray (xray, parent_issue).
---

# Estructura del proyecto automation-flutter

Suite Appium + ScreenPlay para las apps **POS** (`com.redpos.legacy`) y **Service**, en
Flutter/Android. Runner: pytest.

## Arbol principal

```
automation-flutter/
├── conftest.py              # fixtures de sesion: driver, video_recorder, test_config + hooks
├── pytest.ini               # testpaths, markers, logging, junit
├── .env / .env.example      # config de entorno (device, appium, modo, xray)
├── resources/
│   ├── apk/                 # APK local (modo apk)
│   └── test_data/           # default.json, staging.json (datos por ambiente)
├── scripts/                 # run_tests.bat, run_specific_test.bat, upload_results_to_xray.py...
├── artifacts/               # videos, reports (html/json), logs (screenshots) por modulo
└── screenplay/
    ├── actor.py             # clase Actor
    ├── abilities/           # UseAppiumDriver
    ├── interactions/        # acciones atomicas (Click, EnterText, WaitFor...)
    ├── tasks/<modulo>/      # flujos de alto nivel por modulo
    ├── questions/           # lecturas/aserciones
    ├── matchers/            # matchers (uso puntual)
    ├── ui/<modulo>_page.py  # locators por pantalla
    ├── config/              # ConfigurationManager + exceptions
    ├── helpers/             # analisis visual/IA/CV (tests de visuals/)
    └── utils/               # utilidades por dominio (progreso, dashboard, step_debugger)
```

## Modulos con tests (`screenplay/tests/<modulo>/`)

POS: `tienda, anular, compras, deposito, cierre, contingencias, mov_inventario,
refacturacion, reimpresion, reportes, sincronizacion, configuracion`.
Service: `login, clientes, catalogo, ventas, itinerarios, historial_pedidos, metricas,
metricas_agentes, ofertero, flujos_aprobacion, ultima-milla, menu, creditos_cobros`.
Otros: `debug, flujos_prueba, visuals` (visuals se ignora por defecto en pytest.ini).

Cada modulo suele tener capas paralelas: `tests/<modulo>/`, `tasks/<modulo>/`,
`ui/<modulo>_page.py`.

## Convencion de nombres (pytest.ini)

- Archivos: `test_*.py`  |  Clases: `Test*`  |  Funciones: `test_*`.
- `testpaths = screenplay/tests`.

## Fixtures globales (conftest.py de la raiz)

| Fixture | Scope | Que da |
|---|---|---|
| `driver` | session | `webdriver.Remote` de Appium (UiAutomator2). Modo `running_app` (se adjunta a app abierta, `no_reset=True`, `autoLaunch=False`) o `apk`. Al terminar hace `adb force-stop`. |
| `video_recorder` | function | Graba con `adb screenrecord`, guarda en `artifacts/videos/<modulo>/`, nombrado por la key de Xray. **Llamar `video_recorder()` en el `finally`.** |
| `test_config` | session | `ConfigurationManager(os.getenv("TEST_ENV","default"))`. Ver `#test-data-config`. |

Fixtures locales tipicos por modulo (definirlos en el test): `actor`, `timeout`, y los de
datos (`cajero`, `producto_...`, `cliente_nit`, etc.) que leen `test_config`.

## Markers de Xray (pytest.ini + hooks del conftest)

```python
@pytest.mark.xray("TTDEV-12743")        # mapea el test a un ticket de Xray/Jira
@pytest.mark.parent_issue("TTDEV-12582")# vincula a la subtarea/epica padre
```

- `pytest_collection_modifyitems` renombra el test a la key de Xray (salvo placeholders
  repetidos tipo `TTDEV-XXXXX`), guarda `item.xray_key` y nombra el video con esa key.
- En fallo se captura screenshot en `artifacts/logs/<modulo>/screenshots/FAIL_*.png`.
- Al finalizar la sesion se escribe un JSON de resultados en `artifacts/reports/json/<modulo>/`
  que luego sube `scripts/upload_results_to_xray.py`.

## Estructura de un test (resumen)

```python
@pytest.mark.xray("KEY")
@pytest.mark.parent_issue("KEY")
def test_x(driver, video_recorder, test_config, actor, ...):
    try:
        actor.attempts_to(Task1(...))
        actor.attempts_to(Task2(...))
        assert actor.asks(Question(...)), "mensaje"
    except Exception as e:
        pytest.fail(f"test_x fallo: {e}")
    finally:
        video_recorder()
```

> Algunos modulos encadenan estado entre tests (el 2º continua donde quedo el 1º). Es
> intencional; respeta el orden.
