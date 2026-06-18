---
skill_name: deployment_skill
version: "1.0.0"
sector: "e-commerce"
description: >
  Habilidad para desplegar cambios en el ecosistema de forma segura y monitorizada.
  Cubre despliegues en staging y producción, estrategias de rollback, health checks
  y coordinación con los agentes especialistas (db-provisioner, checkout-optimizer).
triggers:
  - "Despliega la nueva versión en staging"
  - "Promueve el despliegue a producción"
  - "Ejecuta un rollback de emergencia"
  - "Verifica el estado post‑despliegue"
dependencies:
  helm: "./charts/"
  health_endpoints: "./04_knowledge/world_model/state.md"
  circuit_breaker: "./04_knowledge/registry.yaml"
tools_required:
  - execute_command
  - kubectl
  - helm
  - query_prometheus
  - slack_notify
---

# Deployment Skill: Despliegue Seguro y Monitorizado

## Propósito

Permitir al agente orquestador:
- Desplegar cambios en el ecosistema siguiendo el ciclo ASA.
- Validar el estado del sistema antes, durante y después del despliegue.
- Ejecutar rollbacks automáticos si se detectan anomalías.
- Coordinar con los agentes especialistas (N2) las validaciones pre y post‑despliegue.

## Estrategias de despliegue soportadas

| Estrategia | Descripción | Cuándo usarla |
|------------|-------------|---------------|
| **Rolling Update** | Reemplazo gradual de pods. Sin downtime. | Cambios de aplicación, configuraciones. |
| **Blue/Green** | Despliegue paralelo, switch de tráfico. | Cambios de esquema de BD, migraciones grandes. |
| **Canary** | Despliegue a un subconjunto de usuarios. | Cambios de alto riesgo, pruebas A/B. |

## Flujo de despliegue

### 1. Pre‑despliegue (fase Hipótesis/Verificar)
- Formular hipótesis de despliegue en `hypothesis.md`:
  - Qué se va a cambiar.
  - Efectos esperados (latencia, errores, uso de recursos).
  - Ventana de monitorización post‑despliegue.
- Validar la hipótesis contra las políticas de seguridad (fase Verificar).

### 2. Despliegue en staging (fase Ejecutar)
- Aplicar el manifiesto o chart Helm en el entorno de staging:
  ```bash
  helm upgrade --install staging-<component> ./charts/<component> --namespace staging
  ```
- Esperar a que todos los pods estén en estado `Running`:
  ```bash
  kubectl wait --for=condition=Ready pods --all -n staging --timeout=300s
  ```
- Ejecutar health check:
  ```bash
  curl -s -o /dev/null -w "%{http_code}" https://staging.example.com/health
  ```
- Si el health check falla: abortar, registrar en `anomaly_notes.md` y notificar.

### 3. Validación en staging (fase Evaluar)
- Delegar al `checkout-optimizer` (N2) la monitorización de métricas durante 15 minutos.
- Delegar al `security-scanner` (N2) un escaneo de vulnerabilidades post‑despliegue.
- Si las métricas cumplen los criterios de la hipótesis → autorizar promoción a producción.
- Si no cumplen → registrar desviación y decidir: ajustar o abortar.

### 4. Despliegue en producción (fase Ejecutar)
- Verificar que la ventana de despliegue está autorizada (según `preferences.yaml`).
- Aplicar el mismo manifiesto/chart en producción:
  ```bash
  helm upgrade --install prod-<component> ./charts/<component> --namespace production
  ```
- Monitorizar en tiempo real durante el despliegue:
  ```bash
  kubectl rollout status deployment/<component> -n production --timeout=300s
  ```

### 5. Post‑despliegue (fase Evaluar)
- Delegar al `log-aggregator` (N1) la ingesta de logs post‑despliegue.
- Delegar al `checkout-optimizer` (N2) la monitorización de SLOs durante 15 minutos.
- Comparar métricas reales con las predicciones de la hipótesis.

### 6. Decisión final (fase Reflexionar)
- Si el despliegue es exitoso → archivar hipótesis como `validated`.
- Si se detectan anomalías → ejecutar rollback y archivar hipótesis como `falsified`.
- Actualizar `learned_rules.yaml` con las lecciones aprendidas.

## Rollback de emergencia

Si se detecta una anomalía grave durante o después del despliegue:

1. Ejecutar rollback inmediato:
   ```bash
   helm rollback prod-<component> --namespace production
   ```
2. Verificar que el estado anterior se ha restaurado.
3. Notificar al operador humano por Slack.
4. Registrar el incidente en `anomaly_notes.md` con trazabilidad completa.

## Integración con el ciclo ASA

| Fase | Acción de despliegue |
|------|---------------------|
| **Hipótesis** | Formular predicciones de efectos del despliegue. |
| **Verificar** | Validar contra políticas de seguridad y contratos semánticos. |
| **Ejecutar** | Aplicar Helm chart en staging/producción. |
| **Evaluar** | Monitorizar métricas, health checks, escaneo de seguridad. |
| **Reflexionar** | Comparar resultados con hipótesis, actualizar modelo causal. |

## Ejemplo de uso

```
Agente orquestador (N3):
  "Voy a desplegar la optimización de índices en staging."

  → Ejecuta helm upgrade en staging.
  → Delegar al checkout-optimizer: "Monitoriza checkout_latency_p95 durante 15 min".
  → checkout-optimizer reporta: p95 bajó de 780ms a 510ms (objetivo: <500ms, cerca).
  → security-scanner reporta: sin vulnerabilidades nuevas.
  → Orquestador decide: promover a producción en la ventana de las 02:00 UTC.
  → Despliegue exitoso. Hipótesis H012 archivada como validada (con desviación menor).
```