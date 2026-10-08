---
inclusion: manual
name: um-negocio
description: Modelo de negocio de Ultima Milla - tipos de ruta, actividades, flujos GLT/AVON, cancelaciones y cierre administrativo.
---

# UM — Negocio

UM cubre la ENTREGA FINAL de pedidos. Solo rutas de **Entregas** (de 4 tipos del
itinerario: Ventas, Cobros, Promociones, Entregas).

## Tipos de ruta
- **Crossdock (padre):** sale del CD con pedidos de varias hijas; se reunen en un punto
  de encuentro para pre-liquidar lo del dia anterior y entregar los pedidos nuevos.
- **Hija:** recibe su carga del padre en el punto de encuentro.
- **Hija independiente:** sin padre; va a bodega a liquidar y recibir pedidos del dia.

## Actividades
- **Crossdock unicas:** entrada a CD, recibir carga de bodega, salida de CD.
- **Crossdock por hija:** Recoleccion Liquidacion, Entrega Pedido.
- **Hija unicas:** recibir carga del camion, inicia ruta, finaliza ruta.
- **Hija por cliente:** segun flujo GLT o AVON.

## Flujos de cliente
- **GLT:** Preparacion de pedido -> Entrega + cobro (metodos de pago en BD).
- **AVON:** Registro de depositos (solo si hay saldo; valida 48h o lo parametrizado) ->
  Entrega de cajas (escanea codigos de barras + evidencia). Sin preparacion.

## Cancelaciones (4 niveles, con reactivacion)
Ruta -> Cliente -> Actividad -> Pedido.

## Cierre administrativo (WEB, GLT)
Liquidacion de ruta -> Cierre de ruta -> Rechazados (devolucion a bodega / reasignacion).
UM en devoluciones llega SOLO hasta la bodega de devoluciones.
