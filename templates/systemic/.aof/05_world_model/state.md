# Estado actual inferido del sistema (Nivel 2: Sistémico)

<!--
  El agente mantiene un estado inferido del sistema que actualiza
  tras cada observación. Esto es parte del Gemelo Digital (Tejido T3).
-->

## Contexto
- Fecha: 2026-06-07 10:00 UTC
- Sector: e-commerce

## Estado operativo
Tráfico moderado (1.2k rpm), sin incidencias activas.
Base de datos principal con CPU al 65%, sin alarmas.
Último despliegue: hace 3 días, estable.

## Variables clave (ver variables.yaml para detalle)
- db_query_latency_p95: 150ms (objetivo: < 50ms)
- db_cpu_usage: 65%
- active_connections: 120

## Sincronización con Gemelo Digital (Tejido T3)
Última sincronización: 2026-06-07 09:55 UTC
Próxima sincronización programada: 2026-06-07 10:05 UTC
Estado de sincronización: OK