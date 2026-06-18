# Anomalías detectadas (Nivel 3: Orquestado)

<!--
  Registro de desviaciones no predichas que activan el módulo
  de reflexión. Cada anomalía se vincula a la hipótesis que la
  detectó (si aplica) y al guardrail que la evaluó.
  Trazabilidad para la Gobernanza Algorítmica (Tejido T1).
-->

## Registro de anomalías

### 2026-06-07 08:30 UTC — Timeout del cache-warmer
- **Descripción**: El agente `cache-warmer` (N1) no respondió en 30 segundos a una solicitud de precalentamiento de caché. Timeout superado.
- **Hipótesis asociada**: No aplica (tarea rutinaria, no requiere hipótesis en N1).
- **Guardrail que la detectó**: Monitor de tiempo de ejecución (Tejido T1).
- **Acción tomada**: Reducción del `trust_score` de 0.83 a 0.78. Marcado del agente como `degraded`. Registro en `registry.yaml`. Notificación al orquestador para investigación.
- **Lección aprendida**: Los timeouts del cache-warmer pueden deberse a picos de latencia en Redis durante la invalidación de caché tras despliegues. Se recomienda aumentar el timeout de 30s a 45s para tareas de precalentamiento.
- **Actualización pendiente**: `policies.yaml` (añadir regla de ajuste de timeout en tareas de caché).

### 2026-06-07 10:15 UTC — Desviación en predicción de latencia
- **Descripción**: La hipótesis H012-2026-06-07 predijo una reducción de la latencia del checkout a 480ms tras añadir un índice parcial en `orders(status)`. El resultado real fue de 510ms (error del 6.25%).
- **Hipótesis asociada**: H012-2026-06-07 (validated, minor deviation).
- **Guardrail que la detectó**: Auditor post‑acción — Comparación de métricas reales vs. hipótesis (Tejido T1).
- **Acción tomada**: La hipótesis se archiva como `validated` (el resultado está dentro del intervalo de confianza del 95%: [430ms, 530ms]). Se actualiza el Grafo Causal Probabilístico con la nueva distribución N(510, 25). Se recomienda investigar optimizaciones adicionales (pooling de conexiones, particionamiento).
- **Lección aprendida**: Los índices parciales son efectivos pero pueden no ser suficientes como única optimización cuando la tabla tiene más de 10M de filas. Se debe considerar una estrategia combinada: índices + pooling + revisión de queries N+1.
- **Actualización pendiente**: `learned_rules.yaml` (refinar regla sobre optimización de índices). `causal_graph.proto` (actualizar distribución de la arista `do(índice) → checkout_latency_p95`).

### Sin anomalías adicionales en las últimas 24 horas.