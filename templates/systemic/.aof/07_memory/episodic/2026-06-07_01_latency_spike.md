# Episodio: Pico de latencia en /search

<!--
  Memoria episódica: registro cronológico de intervenciones.
  El agente almacena aquí cada experiencia con su contexto,
  acción tomada y resultado observado.
-->

## Timestamp
2026-06-07 09:00 UTC

## Observación
El endpoint `/search` alcanzó una latencia de 2s, fuera del rango normal (p95: 150ms).

## Hipótesis formulada
H000-2026-06-07: la tabla `products` carece de índice en la columna `name`.

## Acción tomada
Se añadió índice `idx_products_name` en `products(name)`.

## Resultado
Latencia del endpoint `/search` volvió a 50ms en p95.

## Lección aprendida
Las búsquedas por texto requieren índices específicos. Verificar índices antes de añadir nuevas funcionalidades de búsqueda.