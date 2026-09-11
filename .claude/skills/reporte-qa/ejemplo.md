# Ejemplo — QA de la feature "badge NOVEDAD"

Insumo: `contexto/feature-badge-novedad.md`, criterios de aceptación de la
card:
- back: `items-service` trae `es_novedad: bool`, `true` si `publicado_en` ≤ 3
  días, corte en UTC del server, sin N+1.
- front: `web/index.html` muestra el badge cuando `es_novedad` es `true`.

## Reporte

```
## QA — feature — badge-novedad

**Veredicto: PASS** — 5 tests (5 pasan, 0 fallan)

### Cobertura por criterio
- [x] es_novedad=true si publicado_en <= 3 días — test_es_novedad_dentro_del_corte
- [x] es_novedad=false si publicado_en > 3 días — test_es_novedad_fuera_del_corte
- [x] corte exacto a 3 días (borde) — test_es_novedad_borde_3_dias
- [x] corte en UTC del server, no timezone del cliente — test_es_novedad_usa_utc_server
- [x] no agrega N+1 al listado — test_listado_con_badge_no_hace_n_mas_1
```

Cuatro de los cinco casos salen de derivar el criterio explícito ("≤ 3 días",
"corte en UTC"); el quinto (`no-regresión` del N+1) sale de la causa raíz de
la Clase 5 — cualquier feature nueva sobre el listado se re-testea contra eso.

## Trello

```
[qa] PASS — 5 tests
reporte: https://teg-ai-consulting.atlassian.net/wiki/spaces/.../QA-badge-novedad
```

Card: `QA` → `Done`.
