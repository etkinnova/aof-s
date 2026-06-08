# Sector: Hospital Inteligente

**Descripción:** Este directorio contiene la configuración sectorial de AOF‑S para un **hospital inteligente**, demostrando la aplicabilidad del estándar al sector sanitario. Incluye un `Noosfile` con agentes clínicos, energéticos, de seguridad y logísticos, y skills especializadas para monitorización de signos vitales y optimización energética.

## Agentes del ecosistema

| Agente | Nivel | Función |
|--------|-------|---------|
| `hospital-orchestrator` | 3 (Orquestado) | Coordinación global, cumplimiento de SLOs clínicos y energéticos. |
| `clinical-agent` | 2 (Sistémico) | Monitorización de signos vitales, detección de sepsis, alertas tempranas. |
| `energy-agent` | 2 (Sistémico) | Optimización de climatización (HVAC), baterías y peak shaving. |
| `security-agent` | 2 (Sistémico) | Detección de intrusiones, auditoría HIPAA, escaneo de vulnerabilidades. |
| `logistics-agent` | 2 (Sistémico) | Gestión de camas, suministros y farmacia. |
| `vital-signs-monitor` | 1 (Reactivo) | Ingesta de signos vitales en tiempo real (alta frecuencia). |
| `hvac-local-controller` | 1 (Reactivo) | Control local de climatización por zona. |

## Cómo usar este ejemplo

1. Copia el contenido en tu proyecto:
   ```bash
   cp -r sectors/hospital/.aof .
   cp sectors/hospital/noosfile.yaml .
   ```
2. Personaliza el `noosfile.yaml` con los datos de tu hospital.
3. Aplica el manifiesto:
   ```bash
   noctl apply -f noosfile.yaml
   ```
4. Verifica el estado:
   ```bash
   noctl status
   noctl evaluate ecosystem
   ```

## Cumplimiento normativo

Este ejemplo está diseñado para cumplir con:
- **HIPAA**: trazabilidad completa de decisiones clínicas (Tejido T1).
- **OWASP Top 10 for Agentic Applications (2026)**: mitigaciones declaradas en el `Noosfile`.
