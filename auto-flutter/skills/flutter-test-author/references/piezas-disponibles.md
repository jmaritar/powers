# Piezas ya disponibles (reutilizar antes de crear)

Nombres exactos exportados por los `__init__.py` del proyecto `automation-flutter`.
Antes de escribir una Interaction o Question nueva, busca aqui una equivalente.

## Interactions (`screenplay/interactions/`)

Importan desde `from screenplay.interactions import ...`:

| Pieza | Uso tipico |
|---|---|
| `Click` | `Click.on(locator, timeout)` — tap (usa `mobile: tap` por coordenadas para Flutter). |
| `ClickLeft` | Click desplazado a la izquierda del elemento. |
| `ClickBesideLeft` | Click relativo al costado izquierdo de un elemento. |
| `Enter` | Enviar tecla Enter / confirmar. |
| `EnterText` | `EnterText.into(locator, texto)` — escribe y cierra teclado de forma robusta. |
| `GoBack` | Volver atras (una o varias veces). |
| `HideKeyboard` | `HideKeyboard.now()` — cierra el teclado. |
| `LongPress` | Pulsacion larga. |
| `ScrollDown` | `ScrollDown.quickly()` — scroll hacia abajo. |
| `ScrollUp` | Scroll hacia arriba. |
| `ScrollDownUntil` / `ScrollUpUntil` | Scroll hasta encontrar texto/elemento. |
| `ScrollUntilVisible` / `ScrollUntilVisibleV2` | Scroll hasta que un elemento sea visible. |
| `ScrollFromElement` | Scroll partiendo de un elemento. |
| `SelectDateInPicker` | Seleccionar fecha en el date picker. |
| `SwipeLeft` | Deslizar a la izquierda. |
| `TapAt` / `TapAtCoordinates` / `TapWithOffset` | Taps por coordenadas / con offset. |
| `WaitFor` | `WaitFor.element(locator, timeout)` — espera visibilidad. |
| `UpdateAppiumSettings` | Ajustar settings de Appium en runtime. |
| `safe_click`, `safe_enter` | Helpers "seguros" (funciones, no clases). |

## Questions (`screenplay/questions/`)

Importan desde `from screenplay.questions import ...`. Retornan un valor via
`actor.asks(...)`:

### Genericas / POS
| Pieza | Devuelve |
|---|---|
| `ElementVisible` | bool — elemento visible (acepta lista de locators). |
| `GetElementText` | str — texto de un elemento. |
| `GetAttribute` | str — un atributo (p.ej. `content-desc`). Base para leer valores. |
| `GetTotalAPagar` | float — total a pagar. |
| `GetTotalTarjetas` / `GetTotalCupones` | float — total por metodo de pago (valida si se pasa `expected`). |
| `GetComplementosEnCarrito` | complementos en el carrito. |
| `GetNumeroTransaccion` / `GetTransaccionRefacturada` | numero de transaccion. |
| `GetReciboContent` | contenido del recibo. |
| `ValidarContingenciaEnRecibo` / `ValidarReciboIdentico` | bool — validaciones de recibo. |
| `GetPendientesContingencias` | pendientes de contingencia. |

### Service (login/ventas/itinerarios/metricas/etc.)
| Pieza | Devuelve |
|---|---|
| `ElementVisibleV2`, `ElementNotVisible`, `ElementText`, `ElementEnabled`, `AllElementsVisible`, `ElementClickable`, `ElementNotClickable` | checks de estado de elementos. |
| `PrecioMenorQue`, `EsMenorAlfabeticamente` | comparaciones. |
| `EstadosActividades` y contadores (`ContadorActividadesPendientes`, ...) | estados/conteos de actividades. |
| `GetFirstOrderData`, `ObtenerInformacionVentaConfirmada` | datos de pedidos/ventas. |
| `PrimerDiaConClientes`, `TieneClientes`, `ObtenerNombrePrimerCliente`, ... | itinerarios. |
| `ValorCoberturaMetrica`, `ValorNumericoMetricas` | metricas. |
| `ObtenerCodigoVerificacion`, `CodigosClientesVisibles`, `DatosMarcaVentas`, `TitulosCards`, `ValorNumericoDe`, `ObtenerPrimeraBonificacion`, `DatosNegociacionPuntual`, `ObtenerDescuento`, `ObtenerValorSubtotal`, `ObtenerPrecioUnitario`, `IsExpectedPackage`, `IsExpectedRecipient`, `TextoDe`, `TextBoxEmpty`, `ObtenerUnidadesProducto` | lecturas especificas por dominio. |

## Tasks reutilizables (`screenplay/tasks/`)

Comunes exportadas por `screenplay/tasks/__init__.py`:

| Task | Uso |
|---|---|
| `Login` / `LoginWithCredentials` (alias) | Login POS con `username`, `password`, `fondo_apertura`, `imprimir`, `timeout`. Detecta sesion activa. |
| `RealizarProcesoLogin` | Proceso de login de Service (usuario/contrasena). |
| `Logout` | Cerrar sesion (menu -> scroll -> salir). |
| `ResponderDialogoImpresion` | Responde el dialogo de impresion (imprimir si/no). |
| `Sincronizar`, `ConfirmarSincronizacion` | Sincronizacion. |
| `ToggleInternet` | Encender/apagar internet. |
| `ValidarConfiguracion` | Validar configuracion POS. |
| `NavigateToMenu`, `NavigateToProducts`, `NavigateToMetrics`, `NavigateToHistorialPedidos`, `NavigateToAprobacionesPendientes`, `NavigateMetricasAgentes`, `NavigateToLogin` | Navegacion Service. |
| `OpenMenu`, `ScrollMenu`, `ClickSalir` | Piezas del menu. |

Tasks por modulo viven en subcarpetas (`tasks/tienda/`, `tasks/deposito/`, etc.) y se
importan desde `screenplay.tasks.<modulo>` (p.ej. `from screenplay.tasks.tienda import
NavegaATienda, AgregarProducto, VentaRapida, ...`).

> Si vas a crear una Task/Question/Interaction nueva, revisa primero esta lista y las
> carpetas correspondientes; casi siempre ya hay algo reutilizable.
