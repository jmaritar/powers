# Anatomia del patron ScreenPlay (con ejemplos reales del proyecto)

Rutas relativas a la raiz del proyecto `automation-flutter`.

## El Actor (`screenplay/actor.py`)

Nucleo del patron. Minimalista:

```python
class Actor:
    def __init__(self, name):
        self.name = name
        self.abilities = {}

    def can(self, ability):                 # registra una Ability por nombre de clase
        self.abilities[ability.__class__.__name__] = ability
        return self

    def ability_to(self, ability_class):    # recupera la Ability (ej. UseAppiumDriver)
        return self.abilities.get(ability_class.__name__)

    def attempts_to(self, *tasks):          # ejecuta Tasks/Interactions -> .perform_as(self)
        for task in tasks:
            task.perform_as(self)

    def asks(self, question):               # ejecuta una Question -> .answered_by(self)
        return question.answered_by(self)
```

Contratos de duck-typing:
- **Task / Interaction** -> metodo `perform_as(self, actor)`.
- **Question** -> metodo `answered_by(self, actor)` (retorna un valor).
- **Matcher** -> metodo `matches(self, actor|result)` (retorna bool).

Composicion tipica del actor en un test:

```python
actor = Actor("GT001").can(UseAppiumDriver(driver))
```

## Ability: `UseAppiumDriver` (`screenplay/abilities/use_appium_driver.py`)

Unica ability real; envuelve el `WebDriver` de Appium:

```python
class UseAppiumDriver:
    def __init__(self, driver):
        self.driver = driver
    def get_driver(self):
        return self.driver
```

Dentro de cualquier Task/Interaction/Question se obtiene el driver con:

```python
driver = actor.ability_to(UseAppiumDriver).get_driver()
```

## UI / locators (`screenplay/ui/<modulo>_page.py`)

Clase con atributos XPath (string, lista de fallback, o staticmethod dinamico). Ver detalle
en el steering `#locators-flutter`. Ejemplo:

```python
class TiendaPage:
    TIENDA_BUTTON = "//*[@content-desc='TIENDA']"
    FACTURA_DIALOG = "//*[@content-desc='Dialogo de emision de factura']"

    @staticmethod
    def producto_item(nombre: str) -> str:
        return f"//*[starts-with(@content-desc,'Producto: {nombre}')]"
```

## Interaction (accion atomica) — `screenplay/interactions/`

Clase con `__init__` + factory estatico fluido + `perform_as`. La mayoria ya existen
(`Click.on`, `EnterText.into`, `WaitFor.element`, `HideKeyboard.now`, `ScrollDown.quickly`).
Estructura minima si necesitas una nueva:

```python
from screenplay.abilities.use_appium_driver import UseAppiumDriver

class MiAccion:
    def __init__(self, locator, timeout=10):
        self.locator = locator
        self.timeout = timeout

    @staticmethod
    def on(locator, timeout=10):
        return MiAccion(locator, timeout)

    def perform_as(self, actor):
        driver = actor.ability_to(UseAppiumDriver).get_driver()
        # ... usar WebDriverWait y probar locators en orden si es lista ...
```

## Task (flujo de alto nivel) — `screenplay/tasks/<modulo>/`

Orquesta Interactions y Questions. Ejemplo real corto (`tasks/tienda/navegar_a_tienda.py`):

```python
from screenplay.interactions import Click, WaitFor
from screenplay.ui.tienda_page import TiendaPage

class NavegaATienda:
    def __init__(self, timeout: int = 10):
        self.timeout = timeout

    def perform_as(self, actor):
        try:
            actor.attempts_to(Click.on(TiendaPage.TIENDA_BUTTON, self.timeout))
            actor.attempts_to(WaitFor.element(TiendaPage.FACTURA_DIALOG, self.timeout))
        except Exception as e:
            raise Exception(f"Error al navegar a Tienda: {e}") from e
```

Exporta la Task en el `__init__.py` de su subcarpeta (`screenplay/tasks/<modulo>/__init__.py`).

## Question (lectura/asercion) — `screenplay/questions/`

`answered_by(self, actor)` que retorna un valor. Ejemplo booleano
(`questions/element_visible.py`):

```python
class ElementVisible:
    def __init__(self, locator, timeout=10):
        self.locator = locator if isinstance(locator, list) else [locator]
        self.timeout = timeout

    def answered_by(self, actor):
        driver = actor.ability_to(UseAppiumDriver).get_driver()
        for xpath in self.locator:
            try:
                WebDriverWait(driver, self.timeout).until(
                    EC.visibility_of_element_located((AppiumBy.XPATH, xpath)))
                return True
            except TimeoutException:
                continue
        return False
```

Ejemplo que lee un valor y valida (patron `GetAttribute` + regex, ver
`questions/get_total_tarjetas.py`): lee el `content-desc`, extrae el monto con regex y, si
se pasa `expected`, hace `assert` antes de retornar el float.

## Plantilla de test (capa `tests/`)

Test real minimo con markers de Xray, fixtures y `video_recorder()` en `finally`
(basado en `tests/deposito/test_deposito.py`):

```python
import pytest
from screenplay.actor import Actor
from screenplay.abilities.use_appium_driver import UseAppiumDriver
from screenplay.tasks import LoginWithCredentials
from screenplay.tasks.<modulo> import <MiTask>
from screenplay.questions import ElementVisible
from screenplay.ui.<modulo>_page import <Modulo>Page


@pytest.fixture
def actor(driver):
    return Actor("GT001").can(UseAppiumDriver(driver))

@pytest.fixture
def timeout(test_config):
    return test_config.get_timeout("element_wait")

@pytest.fixture
def cajero(test_config):
    return test_config.get_user("GT001")


@pytest.mark.xray("TTDEV-XXXXX")
@pytest.mark.parent_issue("TTDEV-YYYYY")
def test_mi_caso(driver, video_recorder, test_config, actor, cajero, timeout):
    """
    Descripcion del flujo y resultado esperado.
    """
    try:
        actor.attempts_to(LoginWithCredentials(
            username=cajero.username,
            password=cajero.password,
            fondo_apertura=cajero.fondo_apertura,
            timeout=timeout,
        ))

        actor.attempts_to(<MiTask>(timeout=timeout))

        assert actor.asks(ElementVisible(<Modulo>Page.INDICADOR_EXITO, timeout)), \
            "No se alcanzo el estado esperado"

    except Exception as e:
        pytest.fail(f"test_mi_caso fallo: {e}")
    finally:
        video_recorder()
```

Notas:
- Algunos modulos (ej. `tienda`) tienen tests que **dependen del estado** dejado por el test
  anterior (no re-loguean). Respeta ese orden si el modulo lo usa.
- Los valores calculados por una Task se pasan a la siguiente por atributo (p.ej.
  `agregar = AgregarProducto(...); actor.attempts_to(agregar); ... precio=agregar.precio_unitario`).
