---
inclusion: manual
name: test-data-config
description: Datos de prueba en automation-flutter - ConfigurationManager, archivos resources/test_data/{TEST_ENV}.json, dataclasses y getters (get_user, get_producto_venta, get_deposito, get_timeout...).
---

# Datos de prueba: ConfigurationManager + test_data

Los datos de las pruebas NO se hardcodean en los tests: viven en JSON por ambiente y se leen
via `ConfigurationManager`, expuesto como el fixture `test_config`.

## Archivos

- `resources/test_data/default.json` — ambiente por defecto.
- `resources/test_data/staging.json` — ambiente staging.
- Se elige con la env var `TEST_ENV` (default `default`). Si falta el archivo del ambiente,
  cae a `default.json`. Encoding `utf-8-sig`.

## Secciones del JSON (top-level)

Requeridas: `environment`, `users`, `price_lists`, `timeouts`, `features`.
Opcionales (se leen con `.get`): `compras`, `depositos`, `productos_venta`,
`clientes_tienda`, `metodos_pago_tarjeta`, `metodos_pago_cupon`, `mov_inventario`,
`configuracion_pos`, `cierre`.

Ejemplo (recortado de `default.json`):
```json
{
  "environment": "default",
  "users": [
    { "username": "Cajero", "password": "Cajero123", "role": "cashier", "description": "..." }
  ],
  "price_lists": [ { "name": "Lista General", "code": "LG001", "status": "active", "display_name": "Lista General" } ],
  "depositos": {
    "empresa": "GT001",
    "casos": [
      { "id": "DEP_FELIZ", "no_boleta": "123456", "monto": "1500.00", "banco": "BANCO INDUSTRIAL", "cheque": "" }
    ]
  },
  "timeouts": { "short": 3, "medium": 5, "long": 10, "element_wait": 10, "page_load": 30 }
}
```

## Token especial

El string `"random_6"` en un campo se resuelve a un numero aleatorio de 6 digitos (util para
`no_boleta`/`cheque`).

## Getters del ConfigurationManager

Todos lanzan `DataNotFoundError` con la lista de ids disponibles si no encuentran el dato.

| Getter | Devuelve |
|---|---|
| `get_user("GT001" o "cashier")` | `UserConfig` (busca por username y luego por role). |
| `get_price_list("RETAIL" o indice)` | `PriceListConfig`. |
| `get_timeout("element_wait"|"short"|"medium"|"long"|"page_load")` | int. |
| `get_producto_venta(id)` | `ProductoVentaConfig` (con complementos). |
| `get_cliente_tienda("CF"|"NIT")` | `ClienteTiendaConfig`. |
| `get_metodo_pago_tarjeta(id)` / `get_metodo_pago_cupon(id)` | metodos de pago. |
| `get_deposito(id)` / `get_compra(id)` / `get_mov_inventario(id)` | casos por modulo. |
| `get_configuracion_pos()` / `get_cierre()` | configuraciones. |
| `features` (property) | flags (`imprimir_apertura`, `enable_video_recording`, ...). |

## Uso tipico en un test (via fixtures)

```python
@pytest.fixture
def timeout(test_config):
    return test_config.get_timeout("element_wait")

@pytest.fixture
def cajero(test_config):
    return test_config.get_user("GT001")

@pytest.fixture
def deposito(test_config):
    return test_config.get_deposito("DEP_FELIZ")

def test_deposito(driver, video_recorder, test_config, actor, cajero, deposito, timeout):
    actor.attempts_to(LoginWithCredentials(
        username=cajero.username, password=cajero.password,
        fondo_apertura=cajero.fondo_apertura, timeout=timeout))
    ...
```

## Agregar datos nuevos

1. Anade el caso en `default.json` (y `staging.json` si aplica) en la seccion correcta.
2. Si es una **seccion nueva**, en `screenplay/config/configuration_manager.py`:
   - Crea la `@dataclass` (p.ej. `MiCosaConfig`).
   - Agrega el parser `_parse_mi_cosa()` y llamalo en `__init__`.
   - Expon un getter `get_mi_cosa(id)` con `DataNotFoundError` si no existe.
3. Consume el dato en el test via un fixture que llame al getter.
