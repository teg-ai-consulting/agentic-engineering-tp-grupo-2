---
name: ticket-po
description: >-
  Formatea y crea el ticket de Trello que arranca el pipeline. La usa el PO
  (vos, en el hilo principal de `claude`, no un subagente) para que cada card
  tenga tipo, contexto y criterios de aceptación — el shape que `analista` y
  el guardia esperan. No prioriza ni estima: eso lo decide el PO.
---

Una card mal formada hace que el `analista` adivine y el pipeline salga
torcido. Tu trabajo es que el ticket sea un **contrato legible**, no un
párrafo suelto.

## Antes de crear nada

Pedile al PO lo que falte. Un ticket no sale sin:

- **tipo**: `feature` o `incidente`.
- **título accionable**: `[P?] <servicio> — <qué>`. No "mejorar el listado".
- **contexto**: qué se quiere y **por qué**. Describe el problema/necesidad,
  no la solución. Nada de "decile al `desarrollador` que use tal patrón".
- **criterios de aceptación**: lista verificable (el `qa` tiene que poder
  escribir un test de cada uno). "Anda bien" no es criterio.
- **servicio(s) afectado(s)**: `items-service`, `web/`, etc.
- **owner**: quién revisa (`@usuario`).

Si el PO no sabe un criterio de aceptación, ayudá a derivarlo — no lo dejes en
blanco.

## Formato del cuerpo (feature)

```
tipo: feature · servicio(s): items-service, web/

## Contexto
<qué necesita el negocio y por qué. 2-4 líneas.>

## Criterios de aceptación
- [ ] <observable y testeable>
- [ ] <borde: qué pasa cuando ...>
- [ ] <no-regresión: lo que ya andaba sigue andando>

## Fuera de alcance
- <lo que explícitamente NO entra>

owner: @<quién>
```

## Incidentes

Para `tipo: incidente` espejá `incidente/README.md`: título `[P1] <servicio>
— <síntoma>`, descripción con la alerta, y los adjuntos de evidencia. El
diagnóstico lo hace el `investigador`, no el ticket — no metas una causa raíz
supuesta como si fuera un hecho.

## Crear la card

Con el cuerpo listo, la mecánica de Trello es la misma que usa `crear-card`
(resolver `To-Do` con `get_lists`, `add_card`, agregar el label `tipo: …`) —
la diferencia es quién dispara la creación: ahí es el guardia sobre un
incidente aprobado, acá sos vos como PO sobre una feature nueva.

```
set_active_board(board_id = TRELLO_BOARD_ID)
listas = get_lists()
todo   = next(l.id for l in listas if l.name == "To-Do")
add_card(list_id = todo, name = <título>, desc = <cuerpo de arriba>)
# resolvé "tipo: feature" nombre → id como las listas y agregá el label a la card
```

Devolvé el link de la card y un resumen de una línea.

## Qué NO hace esta skill

- No pone prioridad ni estimación (decisión de PO, no de formato).
- No mueve la card ni pone labels de gate (`aprobado-para-fix`, etc.).
- No le habla al `desarrollador`: describe el qué, no el cómo.
