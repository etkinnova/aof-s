# Pasos de la intervención (Nivel 2: Sistémico)

<!--
  Describe los pasos concretos que el agente ejecutará
  para materializar la hipótesis.
-->

## Hipótesis asociada
H001-2026-06-07

## Pasos
1. Crear migración con el índice parcial en `orders(status)`.
2. Ejecutar migración en staging.
3. Ejecutar `pnpm test:e2e` para validar.
4. Monitorizar latencia y escritura durante 15 minutos.
5. Si las métricas cumplen los criterios, promover a producción.