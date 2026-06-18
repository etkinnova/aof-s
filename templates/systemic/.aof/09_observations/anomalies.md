# Anomalías detectadas (Nivel 2: Sistémico)

<!--
  Registro de desviaciones no predichas que activan el módulo
  de reflexión. Cada anomalía se vincula a la hipótesis que
  la detectó y al guardrail que la evaluó.
-->

## Registro de anomalías

### 2026-06-07 09:05 UTC
- **Descripción**: Pico de latencia en endpoint /search (2s) fuera del horario pico.
- **Hipótesis asociada**: H000-2026-06-07 (validada, sin desviación).
- **Guardrail que la detectó**: monitor de tiempo de ejecución.
- **Acción tomada**: se añadió índice en `product_name`. Latencia volvió a 50ms.
- **Lección aprendida**: verificar índices antes de añadir funcionalidades de búsqueda.

### Sin anomalías adicionales en las últimas 24 horas.