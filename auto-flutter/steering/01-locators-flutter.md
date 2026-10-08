---
inclusion: manual
name: locators-flutter
description: Como escribir locators para automatizar apps Flutter con Appium/UiAutomator2 en automation-flutter - content-desc y hint (no resource-id), listas de fallback y locators dinamicos con staticmethod.
---

# Locators para Flutter (Appium / UiAutomator2)

## Regla fundamental

**Flutter NO mapea `ValueKey` a `resource-id` de Android.** Por eso los locators de este
proyecto se basan en los atributos que Flutter SI expone al arbol de accesibilidad de
Android:

- `content-desc`  ← `Semantics.label` de Flutter (botones, cards, contenedores, dialogos).
- `hint`          ← `Semantics` de campos de texto (inputs).

No inventes `resource-id`. Si no conoces el `content-desc`/`hint` real de un widget, pide un
volcado del page source o el valor exacto; no adivines.

## Donde viven

Un archivo por pantalla en `screenplay/ui/`: `<modulo>_page.py` (POS) o `<modulo>_screen.py`
(Service/menu). Clase con atributos de clase que son XPaths.

## Tres formas de locator

### 1. XPath simple (string)
```python
class TiendaPage:
    TIENDA_BUTTON = "//*[@content-desc='TIENDA']"
    FACTURA_DIALOG = "//*[@content-desc='Dialogo de emision de factura']"
    CAMPO_MONTO_TARJETA = "//*[@hint='Monto de la tarjeta']"
```

### 2. Lista de fallback (varias variantes de texto)
Las Interactions/Questions prueban cada XPath en orden hasta que uno funciona. Util cuando
el texto puede variar (mayusculas, con/sin acento):
```python
class LoginPage:
    LOGIN_BUTTON = [
        "//*[@content-desc='Ingresar']",
        "//*[@text='Ingresar']",
        "//*[contains(@content-desc, 'Ingresar')]",
    ]
    USERNAME_FIELD = [
        "//android.widget.EditText[@resource-id='campo-usuario']",  # cuando SI hay resource-id nativo
        "(//android.widget.EditText)[1]",                           # fallback posicional
    ]
```
> Nota: algunos campos nativos (EditText) si tienen `resource-id`; usalos si existen, pero
> el default para widgets Flutter es content-desc/hint.

### 3. Locator dinamico (staticmethod)
Cuando el locator depende de un dato (nombre de producto, categoria, paso de un dialogo):
```python
class TiendaPage:
    @staticmethod
    def producto_item(nombre: str) -> str:
        return f"//*[starts-with(@content-desc,'Producto: {nombre}')]"

    @staticmethod
    def categoria_item(nombre: str) -> str:
        return f"//*[starts-with(@content-desc,'Categoría {nombre}')]"
```
Uso: `Click.on(TiendaPage.producto_item("COCA COLA"), timeout)`.

## Buenas practicas

- Prefiere `content-desc` exacto (`@content-desc='...'`). Usa `starts-with(...)`/`contains(...)`
  solo cuando el texto tenga sufijos dinamicos o parte variable.
- Manten los labels Flutter **estaticos** (sin ", activo"/", seleccionado") para estabilidad
  entre estados — es una convencion del codigo Flutter del proyecto.
- Para leer un valor (precio, total), suele leerse el `content-desc` completo del widget y
  extraerse con regex (ver `GetAttribute` + `GetTotal*`).
- Campos de texto: hay un locator "contenedor" (por `hint`, para hacer foco) y a veces uno
  "inner" (`.../*[@clickable='true']`) para leer el valor actual.
