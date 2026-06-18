# Criterios de aceptación (Nivel 3: Orquestado)

<!--
  Define las condiciones que deben cumplirse para considerar que
  la misión del ecosistema ha sido completada con éxito.
  Incluye SLOs globales, métricas de rendimiento, criterios de
  seguridad y validación de hipótesis.
-->

## SLOs Globales

| SLO | Objetivo | Ventana | Prioridad |
|-----|----------|---------|-----------|
| checkout_latency_p95 | < 500ms | 7d | Crítico |
| ecosystem_uptime | >= 99.95% | 30d | Crítico |
| conversion_rate | >= baseline * 0.99 | 7d | Crítico |
| mean_time_to_resolution | < 15min | 30d | Alto |
| hypothesis_validation_rate | > 85% | 90d | Alto |
| token_efficiency_ratio | > 70% | 30d | Medio |

## Validación de hipótesis

- [ ] Hipótesis activa registrada en `hypotheses/active/` con efectos de 1º, 2º y 3º orden.
- [ ] Hipótesis validada por el motor neuro‑simbólico (fase Verificar) antes de la ejecución.
- [ ] Resultado comparado con predicciones en la fase Evaluar.
- [ ] Hipótesis archivada como `validated` o `falsified` tras la reflexión.
- [ ] Lecciones extraídas registradas en `learned_rules.yaml` o `domain_facts.md`.

## Criterios de seguridad

- [ ] Toda acción en producción requiere hipótesis registrada y verificación previa.
- [ ] No se han introducido vulnerabilidades críticas (OWASP Top 10 Agentic).
- [ ] Las comunicaciones entre agentes utilizan mTLS con identidades SPIFFE verificables.
- [ ] Las políticas OPA declaradas en el `Noosfile` están aplicadas y sin violaciones.
- [ ] El ledger de auditoría contiene trazabilidad completa de las decisiones autónomas.

## Criterios de despliegue

- [ ] Todo cambio en producción ha sido validado en staging durante al menos 15 minutos.
- [ ] No se despliega en viernes, sábados ni vísperas de festivo.
- [ ] El rollback automático se ha probado y está operativo.
- [ ] Los health checks post‑despliegue responden 200 en menos de 30s.

## Criterios de memoria y aprendizaje

- [ ] El Grafo Causal Probabilístico se ha actualizado tras cada ciclo de reflexión.
- [ ] Las reglas semánticas con `confidence < 0.5` han sido marcadas para revisión.
- [ ] Las políticas procedurales reflejan las lecciones aprendidas en los últimos 30 días.
- [ ] La memoria meta‑cognitiva registra el rendimiento de las estrategias de razonamiento utilizadas.

## Criterios de ecosistema

- [ ] Todos los agentes registrados en `registry.yaml` tienen `trust_score >= 0.70`.
- [ ] No hay agentes en estado `degraded` o `failed` sin plan de recuperación.
- [ ] La memoria colectiva (evaluaciones de N1) está siendo consumida por agentes N2 y N3.
- [ ] El `Noosfile` refleja el estado deseado del ecosistema (`noctl apply` sin desviaciones).