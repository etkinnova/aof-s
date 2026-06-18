# Hipótesis de intervención (Nivel 2: Sistémico)

<!--
  NUEVO en v1.0: estructura para registrar hipótesis antes de actuar.
  Toda acción debe tener una hipótesis registrada (Principio 3).
  Esto implementa la Gobernanza Algorítmica (Tejido T1).
-->

## Identificador
H001-2026-06-07

## Necesidad
Reducir la latencia p95 de las consultas a la tabla `orders` por debajo de 50ms.

## Acción propuesta
Añadir índice parcial en la columna `status` de la tabla `orders` para filtrar por pedidos activos.

## Efectos previstos

### Efecto de 1er orden (deseado inmediato)
Las consultas SELECT con filtro `WHERE status = 'active'` bajarán de 150ms a 20ms en p95.

### Efecto de 2º orden (resistencias o reacciones del sistema a corto plazo)
- Ligero aumento en el tiempo de escritura (INSERT/UPDATE) por mantenimiento del índice: estimado +5ms por operación.
- Posible contención en la tabla durante la creación del índice en caliente.

## Métricas de validación
- p95 de queries sobre `orders` < 50ms en staging tras despliegue.
- Tiempo de escritura no superior a +10ms respecto a la línea base.
- Sin errores 500 en los primeros 15 minutos post‑despliegue.

## Trazabilidad (Gobernanza Algorítmica)
- Guardrail pre‑acción: verificado contra `constitution.md` (no viola reglas).
- Auditor post‑acción: pendiente de registro en `observations/feedback.yaml`.