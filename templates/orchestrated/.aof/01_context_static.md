# Contexto estático — Nivel 3 (Orquestado)

<!--
  Información inmutable del ecosistema. El agente orquestador la lee en cada
  sesión para entender el stack, la infraestructura y las reglas globales.
  Este contexto es compartido por todos los agentes del sub‑Noosistema.
-->

## Stack del Ecosistema
- **Orquestación**: Kubernetes 1.30+, K3s (edge), Helm
- **Lenguajes**: Node.js 22, TypeScript 5.7, Python 3.12
- **Bases de datos**: PostgreSQL 16, Redis 7, MongoDB 7
- **Mensajería**: Kafka 3.6, NATS 2.10
- **Monitoreo**: Prometheus, Grafana, Loki, OpenTelemetry

## Comandos Globales
- `kubectl apply -f <manifest>`
- `helm upgrade --install <release> <chart>`
- `noctl apply -f noosfile.yaml`

## Reglas no negociables
1. Todo despliegue en producción requiere aprobación del orquestador y validación previa en staging.
2. Las migraciones de base de datos deben ser transaccionales y con plan de rollback documentado.
3. No se exponen secretos en logs, observaciones ni respuestas a agentes.
4. Cumplimiento PCI DSS obligatorio en el flujo de pago.
5. Todo nuevo agente debe registrarse en `registry.yaml` con identidad SPIFFE.

## Documentación indexada
| Recurso | Ruta |
|----------|------|
| Arquitectura del ecosistema | docs/architecture/README.md |
| Políticas de seguridad | docs/security/policies.md |
| Guía de despliegue | docs/deploy/README.md |
| Modelo causal del checkout | docs/causal/checkout-model.md |
| Runbooks de incidentes | docs/runbooks/ |