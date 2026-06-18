# diagnostic_skill

<!--
  Habilidad modular para diagnosticar problemas de rendimiento.
  El agente carga esta skill cuando necesita analizar métricas
  y encontrar cuellos de botella.
-->

## Propósito
Analizar métricas de base de datos, identificar queries lentas y proponer optimizaciones.

## Herramientas
- `query_database`: ejecutar consultas de diagnóstico.
- `execute_command`: ejecutar herramientas de profiling.

## Flujo
1. Ejecutar `EXPLAIN ANALYZE` sobre las queries lentas.
2. Identificar si faltan índices o si hay contención de locks.
3. Proponer optimización (índice, reescritura de query, particionamiento).
4. Validar en staging antes de producción.