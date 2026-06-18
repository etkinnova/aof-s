# Hechos del dominio (Nivel 3: Orquestado)

<!--
  Memoria semántica: hechos verificados sobre el dominio de e-commerce.
  El agente orquestador consulta estos hechos durante la fase de Razonar
  para contextualizar sus decisiones. Se actualizan tras cada ciclo de
  reflexión cuando se descubre nueva información.
-->

## Infraestructura y despliegue
- El clúster de producción tiene 8 nodos worker (16 vCPUs, 64 GB RAM cada uno).
- El clúster de staging tiene 4 nodos worker (8 vCPUs, 32 GB RAM cada uno).
- La ventana de despliegue en producción es de lunes a jueves, 02:00‑04:00 UTC.
- No se despliega en viernes, sábados ni vísperas de festivo por política operacional.
- El auto‑scaler está configurado para añadir nodos cuando la CPU supera el 70% durante 5 minutos.
- Helm es la herramienta de despliegue estándar para todos los servicios.

## Checkout y base de datos
- La pasarela de pago externa (Stripe) tiene un SLA del 99.5% de disponibilidad.
- El checkout tiene picos de tráfico de 12:00 a 14:00 UTC y de 18:00 a 20:00 UTC.
- La tabla `orders` tiene actualmente ~15 millones de filas y crece a un ritmo de ~500k filas/día.
- PostgreSQL 16 está configurado con `max_connections = 200` (actual), con plan de ampliar a 300.
- Las consultas de búsqueda representan el 40% de la carga total de la base de datos.
- El tiempo medio de una migración de esquema en producción es de 45 segundos.

## Seguridad
- PCI DSS v4.0 aplica a todo el flujo de pago (checkout, billing, refunds).
- El escaneo de vulnerabilidades se ejecuta automáticamente cada 24 horas.
- Las credenciales de base de datos se rotan cada 90 días por política de seguridad.
- El service mesh (Istio) aplica mTLS en todas las comunicaciones entre servicios.
- Todos los agentes del ecosistema tienen identidad SPIFFE verificable.
- El agente `security-scanner` reporta las vulnerabilidades críticas en menos de 1 hora.

## Clientes y negocio
- La tasa de conversión promedio es del 3.2%, con una línea base de 3.17% (mínimo aceptable).
- El 70% de los usuarios accede desde dispositivos móviles.
- Los picos de tráfico estacionales ocurren en Black Friday (noviembre) y rebajas de enero.
- El carrito medio tiene 3.2 productos.
- La tasa de abandono del checkout es del 65% (objetivo: reducir a <60%).

## Energía y recursos
- El centro de datos consume una media de 4.2 MW, con picos de 5.1 MW en horas punta.
- Los paneles solares generan hasta 540 kWp en horas centrales del día.
- Las baterías tienen una capacidad total de 1 MWh y se descargan en horario punta (18:00‑22:00).
- El PUE (Power Usage Effectiveness) objetivo es < 1.2.
- La refrigeración representa el 35% del consumo energético total del centro de datos.

## Agentes del ecosistema
- El `db-provisioner` (N2) es el único agente autorizado para ejecutar migraciones de base de datos.
- El `checkout-optimizer` (N2) ejecuta tests A/B sobre el flujo de checkout.
- El `security-scanner` (N2) escanea vulnerabilidades en dependencias, configuraciones y código.
- El `log-aggregator` (N1) ingiere y normaliza logs de todos los servicios.
- El `cache-warmer` (N1) precalienta las cachés CDN y Redis tras cada despliegue.
- El orquestador (N3) es el único agente autorizado para crear nuevos agentes en el ecosistema.

## Integración con el Gemelo Digital Unificado (Tejido T3)
- Estos hechos se sincronizan con el Gemelo Digital Unificado (dg://ecommerce/ecosystem).
- El `state.md` del orquestador es el fragmento del gemelo digital que gestiona directamente.
- Las variables en `variables.yaml` se actualizan desde el gemelo digital cada 5 minutos.