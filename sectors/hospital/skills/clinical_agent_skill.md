---
skill_name: clinical_agent_skill
version: "1.0.0"
sector: "hospital"
description: >
  Habilidad especializada para monitorización de signos vitales, detección de
  patrones anómalos y alertas tempranas en entornos hospitalarios. Cumple con
  HIPAA para el manejo de datos de pacientes. Implementa el ciclo ASA completo:
  formula hipótesis de deterioro, verifica contra umbrales clínicos, ejecuta
  alertas monitorizadas y evalúa la precisión de las predicciones.
triggers:
  - "Monitoriza los signos vitales del paciente en cama 5"
  - "Revisa las tendencias de tensión arterial en la UCI"
  - "¿Hay algún paciente con signos de sepsis temprana?"
  - "Genera informe de constantes vitales para el turno saliente"
  - "Evalúa el riesgo de deterioro en las próximas 6 horas"
dependencies:
  time_series_db: "prometheus"
  vital_signs_schema: "../../../schemas/vital_signs_request.json"
  hipaa_audit_log: "loki"
  clinical_thresholds: "./clinical_thresholds.yaml"
tools_required:
  - read_file
  - execute_command
  - query_prometheus
  - send_alert
compliance:
  - "HIPAA"
  - "Esquema Nacional de Seguridad (ENS)"
---

# Clinical Agent Skill: Monitorización de Signos Vitales y Alertas Tempranas

## Propósito

Permitir al agente clínico (N2):
- Monitorizar signos vitales en tiempo real desde dispositivos IoT médicos (Capa C1 del Holosistema TIC).
- Detectar patrones anómalos indicativos de deterioro clínico (sepsis, fallo cardíaco, distress respiratorio).
- Formular hipótesis de deterioro con predicciones temporales y niveles de severidad.
- Generar alertas tempranas con trazabilidad completa para auditoría HIPAA (Tejido T1).
- Coordinar con el orquestador (N3) y el agente energético para optimizar recursos sin comprometer la seguridad del paciente.

## Flujo de monitorización (Ciclo ASA)

### 1. Arranque
- Cargar `clinical_thresholds.yaml` con los umbrales de signos vitales por unidad clínica (UCI, planta, urgencias).
- Verificar la identidad SPIFFE y la conexión a la malla de servicios (Tejido T2).
- Sincronizar con el Gemelo Digital Unificado (Tejido T3) para obtener el estado actual de ocupación y programación quirúrgica.

### 2. Razonar (contextualización + inferencia)
- Recuperar de la memoria episódica los episodios similares (pacientes con patrones de deterioro comparables).
- Consultar `learned_rules.yaml` para reglas clínicas extraídas de experiencias previas.
- Ingerir signos vitales desde Prometheus (frecuencia: 1s para FC, 5s para SpO2, 15min para TA).

### 3. Hipótesis
- Formular hipótesis de deterioro cuando se detecten desviaciones:
  ```markdown
  ## Hipótesis clínica (ejemplo)
  - Paciente: cama 3 UCI (anonimizado)
  - Observación: SpO2 bajando de 96% a 91% en 45 minutos
  - Predicción: si la tendencia continúa, SpO2 < 88% en ~30 minutos
  - Efecto 1º: hipoxemia moderada (SpO2 85-88%)
  - Efecto 2º: posible activación del protocolo de respuesta rápida
  - Confianza: 0.82 (basada en 143 episodios similares)
  ```

### 4. Verificar (validación neuro-simbólica)
- Validar la hipótesis contra los umbrales clínicos configurados.
- Verificar que la alerta no viola restricciones de HIPAA (datos anonimizados en logs).
- Confirmar que el canal de notificación está autorizado por las políticas OPA.

### 5. Ejecutar
- Generar alerta según el nivel de severidad:
  - **INFO:** desviación leve. Registrar en log, no notificar.
  - **WARNING:** desviación moderada. Notificar a enfermera de planta (Slack/Telegram).
  - **CRITICAL:** riesgo vital inminente. Notificar al médico de guardia y activar protocolo de respuesta rápida.
- Registrar la alerta en el ledger de auditoría con trazabilidad completa.

### 6. Evaluar
- Comparar el resultado real con la hipótesis formulada.
- Registrar en `feedback_log.yaml`:
  ```yaml
  - timestamp: "2026-06-07 14:30:00Z"
    metric: spo2_trend_uci_3
    hypothesis: "H017-2026-06-07"
    result: "validated"
    deviation: "SpO2 real 87% vs predicho 88% (error 1.1%)"
  ```

### 7. Reflexionar
- Actualizar la memoria semántica con la nueva evidencia.
- Refinar las políticas procedurales: ajustar umbrales de alerta si la tasa de falsos positivos supera el 15%.
- Reportar al orquestador (N3) las métricas de precisión para actualizar el `trust_score`.

## Integración con el ecosistema hospitalario

- **Con el energy-agent (N2):** si el clinical-agent predice un pico de ingresos, el energy-agent pre-enfría las habitaciones correspondientes sin comprometer las condiciones clínicas.
- **Con el hospital-orchestrator (N3):** reporta métricas de falsos positivos y tiempos de respuesta. El orquestador ajusta el `trust_score` del agente clínico en el `registry.yaml`.
- **Con el gemelo digital (Tejido T3):** el estado fisiológico de los pacientes (anonimizado) se refleja en el gemelo digital para simulaciones "what-if" a nivel de planta.

## Umbrales clínicos de referencia

| Signo vital | Normal | WARNING | CRITICAL |
|-------------|--------|---------|----------|
| SpO2 (%) | > 95% | 90-95% | < 90% |
| Frecuencia cardíaca (lpm) | 60-100 | 50-60 / 100-120 | < 50 / > 120 |
| Tensión arterial sistólica (mmHg) | 100-140 | 90-100 / 140-160 | < 90 / > 160 |
| Temperatura (°C) | 36.0-37.5 | 35.5-36.0 / 37.5-38.5 | < 35.5 / > 38.5 |
| Frecuencia respiratoria (rpm) | 12-20 | 20-25 | < 12 / > 25 |

## Ejemplo de uso

```
Agente clínico (N2, fase Razonar):
  "Monitorizando signos vitales en UCI. 12 pacientes activos."

Agente clínico (fase Hipótesis):
  "Paciente cama 3 UCI: descenso SpO2 96→91% en 45 min.
   Hipótesis H017-2026-06-07: posible distress respiratorio.
   Predicción: SpO2 < 88% en ~30 min si no se interviene.
   Confianza: 0.82 (143 episodios similares)."

Agente clínico (fase Verificar):
  "Validación neuro-simbólica: hipótesis aprobada.
   Umbral WARNING alcanzado. No viola restricciones HIPAA."

Agente clínico (fase Ejecutar):
  "Enviando alerta WARNING a Slack#enfermeria-uci:
   'Paciente cama 3 UCI: descenso SpO2 96→91% en 45 min.
   Sugerir gasometría arterial. Hipótesis: H017-2026-06-07.'"

Agente clínico (fase Evaluar, 30 min después):
  "Resultado: SpO2 real 87% (predicho 88%, error 1.1%).
   Hipótesis H017 validada. Archivando en validated/."

Agente clínico (fase Reflexionar):
  "Actualizando memoria semántica: confianza de regla 'descenso SpO2 > 5%
   en < 1h → distress respiratorio' incrementada de 0.82 a 0.84."
```