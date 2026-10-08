---
inclusion: manual
name: screenplay-pattern
description: Patron ScreenPlay usado en el proyecto automation-flutter (Actor, Abilities, Tasks, Interactions, Questions, Matchers) y los contratos de cada capa para escribir pruebas Appium sobre Flutter.
---

# Patron ScreenPlay (automation-flutter)

La suite organiza las pruebas Appium con el patron ScreenPlay. La idea: un **Actor** con
**Abilities** ejecuta **Tasks** (compuestas de **Interactions**) y responde **Questions**,
apoyandose en locators de la capa **ui/**.

## Capas y contratos

| Capa | Carpeta | Contrato (metodo) | Rol |
|---|---|---|---|
| Actor | `screenplay/actor.py` | `attempts_to` / `asks` / `can` / `ability_to` | Ejecuta el flujo. |
| Ability | `screenplay/abilities/` | `get_driver()` | Da acceso al driver de Appium. |
| Interaction | `screenplay/interactions/` | `perform_as(self, actor)` | Accion atomica (click, type...). |
| Task | `screenplay/tasks/` | `perform_as(self, actor)` | Flujo de alto nivel (login, pagar...). |
| Question | `screenplay/questions/` | `answered_by(self, actor)` | Lee/valida estado; retorna valor. |
| Matcher | `screenplay/matchers/` | `matches(self, ...)` | Asercion booleana (uso puntual). |
| UI/locators | `screenplay/ui/` | atributos XPath | Donde estan los elementos. |

## El Actor

```python
actor = Actor("GT001").can(UseAppiumDriver(driver))
actor.attempts_to(NavegaATienda(timeout=10))          # ejecuta Tasks/Interactions
visible = actor.asks(ElementVisible(TiendaPage.CARRITO, 10))   # ejecuta una Question
```

- `attempts_to(*tasks)` llama `task.perform_as(self)` en orden.
- `asks(question)` llama `question.answered_by(self)` y retorna su valor.
- `can(ability)` registra una ability; `ability_to(Clase)` la recupera.

## Obtener el driver dentro de una pieza

```python
from screenplay.abilities.use_appium_driver import UseAppiumDriver
driver = actor.ability_to(UseAppiumDriver).get_driver()
```

## Como componer

- Una **Task** casi nunca toca el driver directo: llama Interactions
  (`actor.attempts_to(Click.on(...))`) y Questions (`actor.asks(ElementVisible(...))`).
- Una **Interaction/Question** si toca el driver (via la ability) porque es la capa atomica.
- El **test** solo compone Tasks + aserciones con Questions; no arma XPaths ni logica de bajo
  nivel.

## Reglas de estilo del proyecto

- Nombres de Tasks en español y como accion: `NavegaATienda`, `AgregarProducto`,
  `CompletarFormularioDeposito`.
- Interactions con factory estatico fluido: `Click.on(...)`, `EnterText.into(...)`,
  `WaitFor.element(...)`, `HideKeyboard.now()`, `ScrollDown.quickly()`.
- Questions de lectura: prefijo `Get...`/`Obtener...`; de check: `Element...`.
- Toda Task nueva se **exporta** en el `__init__.py` de su subcarpeta.
- Reutiliza piezas existentes antes de crear nuevas.

## Import util

`from screenplay import Actor` y `from screenplay.abilities import UseAppiumDriver` funcionan
como fachada; tambien se pueden importar de sus modulos concretos
(`from screenplay.actor import Actor`).
