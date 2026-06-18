# Pasos de la intervención actual (Nivel 3: Orquestado)

<!--
  Plan táctico del orquestador para la intervención en curso.
  Derivado de la hipótesis activa H012-2026-06-07.
  El orquestador coordina a los agentes especialistas (N2) y
  reactivos (N1) para ejecutar estos pasos de forma monitorizada.
-->

## Hipótesis asociada
H012-2026-06-07 (active)

## Resumen de la intervención
Reducir la latencia p95 del checkout de 780ms a <500ms mediante
optimización de índices en la tabla `orders` y ajuste del pooling
de conexiones de la base de datos.

## Pasos

### Paso 1: Simulación contrafactual
- **Responsable:** Orquestador (N3)
- **Acción:** Ejecutar simulación con el Grafo Causal Probabilístico
  para predecir efectos de 1º, 2º y 3º orden de la intervención.
- **Herramienta:** `simulation_skill.md` + `causal_graph.proto`
- **Resultado esperado:** Predicción de latencia post-intervención
  con intervalo de confianza al 95%.
- **Estado:** ✅ Completado (2026-06-07 09:30)

### Paso 2: Validación neuro-simbólica
- **Responsable:** Orquestador (N3) — fase Verificar
- **Acción:** Validar la hipótesis contra las reglas de seguridad
  y los contratos semánticos (PCI DSS, OPA).
- **Herramienta:** Motor CLIPS + `verification_rules.clp`
- **Resultado esperado:** Hipótesis autorizada o rechazada.
- **Estado:** ✅ Completado (2026-06-07 09:31) — Autorizada

### Paso 3: Crear migración de índices
- **Responsable:** db-provisioner (N2)
- **Acción:** Crear migración con índice parcial en `orders(status)`
  y ajustar el pool de conexiones (max_connections: 200 → 300).
- **Herramienta:** `pnpm migrate:create`
- **Resultado esperado:** Migración lista para ejecutar en staging.
- **Estado:** ⏳ En progreso (delegado a db-provisioner a las 09:32)

### Paso 4: Desplegar en staging
- **Responsable:** Orquestador (N3)
- **Acción:** Aplicar migración en el entorno de staging.
- **Herramienta:** `helm upgrade --install staging-db ./charts/db`
- **Resultado esperado:** Migración aplicada sin errores.
- **Estado:** ☐ Pendiente (esperando Paso 3)

### Paso 5: Validar métricas en staging
- **Responsable:** checkout-optimizer (N2) + log-aggregator (N1)
- **Acción:** Monitorizar latencia, escritura y errores durante 15 minutos.
- **Herramienta:** `query_prometheus`, `query_loki`
- **Resultado esperado:** p95 < 50ms, escritura +5ms, sin errores 500.
- **Estado:** ☐ Pendiente (esperando Paso 4)

### Paso 6: Promover a producción
- **Responsable:** Orquestador (N3)
- **Acción:** Aplicar migración en producción durante la ventana
  de bajo tráfico (2026-06-08 02:00 UTC).
- **Herramienta:** `helm upgrade --install prod-db ./charts/db`
- **Resultado esperado:** Despliegue exitoso, sin tiempo de inactividad.
- **Estado:** ☐ Pendiente (esperando Paso 5)

### Paso 7: Reflexión y cierre
- **Responsable:** Orquestador (N3)
- **Acción:** Comparar resultados reales con la hipótesis, actualizar
  el Grafo Causal, extraer reglas semánticas y refinar políticas.
- **Herramienta:** Ciclo ASA — fase 7 (Reflexionar)
- **Resultado esperado:** Hipótesis validada o falsada, modelo actualizado.
- **Estado:** ☐ Pendiente (esperando Paso 6)

## Notas del orquestador
- El cache-warmer (N1) sigue en estado `degraded`. No es bloqueante
  para esta intervención, pero se debe investigar en paralelo.
- La ventana de despliegue en producción es a las 02:00 UTC del 08-jun.
  Si el Paso 5 no se completa antes de las 01:30 UTC, se pospone
  al día siguiente.