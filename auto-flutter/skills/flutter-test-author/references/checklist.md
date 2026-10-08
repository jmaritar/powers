# Checklist: "prueba lista"

Antes de dar por terminada una prueba nueva, verifica:

## Locators (capa `ui/`)
- [ ] Cada elemento usa `content-desc` o `hint` reales (NO `resource-id` inventado).
- [ ] Se usan listas de fallback donde el texto puede variar.
- [ ] Locators dinamicos como `@staticmethod` (nombre de producto, categoria, etc.).
- [ ] El locator vive en la clase `<Modulo>Page`/`<Modulo>Screen` correcta.

## Tasks y Questions
- [ ] Se reutilizaron piezas existentes cuando existian (ver `piezas-disponibles.md`).
- [ ] Las Tasks nuevas tienen `perform_as(self, actor)` y try/except con mensaje claro.
- [ ] Las Tasks nuevas estan exportadas en el `__init__.py` de su subcarpeta.
- [ ] Las Questions nuevas tienen `answered_by(self, actor)` y retornan un valor util.
- [ ] Timeouts vienen de `test_config.get_timeout(...)` (no hardcodeados sin razon).

## Datos de prueba
- [ ] Los datos del caso estan en `resources/test_data/default.json` (y `staging.json` si aplica).
- [ ] Se accede a ellos con el getter correcto del `ConfigurationManager`.
- [ ] Si es una seccion nueva de datos, tiene dataclass + parser + getter.

## El test (capa `tests/`)
- [ ] Archivo en `screenplay/tests/<modulo>/test_<modulo>.py`.
- [ ] Marcado con `@pytest.mark.xray("KEY")` (real o placeholder acordado).
- [ ] Marcado con `@pytest.mark.parent_issue("KEY")` cuando corresponde.
- [ ] Firma con los fixtures necesarios: `driver, video_recorder, test_config, actor, ...`.
- [ ] Cuerpo con `actor.attempts_to(...)` y aserciones con `assert actor.asks(...)`.
- [ ] Todo dentro de try/except que hace `pytest.fail(...)` con mensaje.
- [ ] `video_recorder()` llamado en el `finally`.
- [ ] Docstring que describe el flujo y el resultado esperado.
- [ ] Respeta la dependencia de orden del modulo si existe (no re-loguear cuando no toca).

## Ejecucion (ver `#ejecucion`)
- [ ] Se indica el comando (`scripts\run_tests.bat <modulo>` o `poetry run pytest ...`).
- [ ] Precondiciones claras: emulador en `adb devices`, Appium arriba, app abierta si
      `TEST_MODE=running_app`, `.env` configurado.
- [ ] NO se lanza el runner como proceso de larga duracion desde el agente.
