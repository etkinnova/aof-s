---
skill_name: energy_optimization_skill
version: "1.0.0"
sector: "hospital"
description: >
  Habilidad especializada para optimizar el consumo energético hospitalario
  mediante control predictivo de climatización (HVAC), gestión de baterías
  y coordinación con el agente clínico para no afectar las condiciones
  asistenciales. Implementa el ciclo ASA completo: formula hipótesis de
  ahorro, verifica contra restricciones clínicas y evalúa la precisión
  de las predicciones.
triggers:
  - "Optimiza el consumo energético del Edificio A"
  - "Predice la demanda energética para las próximas 24 horas"
  - "Coordina el pre‑enfriamiento de quirófanos antes del pico de demanda"
  - "Gestiona la carga y descarga de baterías"
  - "Evalúa el ahorro energético de la semana"
dependencies:
  time_series_db: "prometheus"
  weather_api: "https://api.weather.gov"
  energy_meters_schema: "../../../schemas/energy_meters.json"
  clinical_agent: "agent://clinical-agent"
tools_required:
  - read_file
  - execute_command
  - query_prometheus
  - call_weather_api
  - send_recommendation
compliance:
  - "ISO 50001"
  - "Esquema Nacional de Seguridad (ENS)"
---

# Energy Optimization Skill: Eficiencia Energética Hospitalaria

## Propósito

Permitir al agente energético (N2):
- Predecir la demanda energética del hospital con 24 horas de antelación.
- Optimizar la climatización (HVAC) por zonas, respetando las condiciones clínicas.
- Gestionar la carga/descarga de baterías para minimizar el consumo en horas pico.
- Coordinar con el clinical-agent (N2) para pre‑enfriar zonas sin afectar a pacientes.
- Formular hipótesis de ahorro con predicciones cuantitativas y verificar su cumplimiento.

## Flujo de optimización (Ciclo ASA)

### 1. Arranque
- Cargar `variables.yaml` del modelo del mundo: consumo histórico, capacidad de baterías, generación fotovoltaica.
- Verificar la identidad SPIFFE y la conexión a la malla de servicios (Tejido T2).
- Sincronizar con el Gemelo Digital Unificado (Tejido T3) para obtener el estado actual de ocupación y climatización por zona.

### 2. Razonar (contextualización + inferencia)
- Recuperar de la memoria episódica los patrones de consumo de días similares (laborable, fin de semana, estación).
- Consultar `learned_rules.yaml` para reglas energéticas (ej: "pre‑enfriamiento matutino reduce pico de demanda un 15%").
- Ingerir datos de Prometheus (consumo eléctrico por cuadro, temperatura por zona, estado de baterías).
- Consultar predicción meteorológica (temperatura exterior, humedad, radiación solar) para las próximas 24 horas.

### 3. Hipótesis
- Formular hipótesis de ahorro cuando se detecten oportunidades:
  ```markdown
  ## Hipótesis energética (ejemplo)
  - Predicción de demanda: pico de 320 kW a las 14:00 (temperatura exterior 35°C).
  - Generación solar prevista: 80 kW en horas centrales.
  - Ocupación: consultas externas bajan a las 14:00 (fin turno matutino).
  - Propuesta: pre‑enfriar consultas externas (22→20°C) entre 13:00 y 14:00,
    luego liberar climatización. Combinado con descarga de baterías en punta.
  - Efecto 1º previsto: reducción de 45 kW en el pico de las 14:00 (14% de ahorro).
  - Efecto 2º previsto: ligero aumento de confort térmico en consultas (máx 24°C a las 15:00).
  - Confianza: 0.89 (basada en 45 episodios similares).
  ```

### 4. Verificar (validación neuro-simbólica)
- Validar la hipótesis contra las restricciones clínicas:
  - ¿El pre‑enfriamiento afecta a zonas críticas (UCI, quirófanos)? → No, solo consultas externas.
  - ¿Se compromete la autonomía de baterías para equipos de soporte vital? → No, SOC mínimo 20% garantizado.
- Verificar que la propuesta cumple la política energética (ISO 50001).

### 5. Ejecutar
- Solicitar al clinical-agent confirmación de que no hay restricciones clínicas en la zona afectada.
- Programar el HVAC: encender a las 13:00, apagar a las 14:00.
- Programar la descarga de baterías: inyectar 200 kW a la red interna de 18:00 a 22:00.
- Registrar la acción en el ledger de auditoría.

### 6. Evaluar
- Comparar el resultado real con la hipótesis formulada.
- Registrar en `feedback_log.yaml`:
  ```yaml
  - timestamp: "2026-06-07 15:00:00Z"
    metric: peak_demand_reduction_kw
    value: 42
    unit: kW
    hypothesis: "H018-2026-06-07"
    result: "validated"
    deviation: "-3 kW (predicho 45 kW, real 42 kW, error 6.7%)"
    notes: "Temperatura en consultas alcanzó 23.8°C, dentro del rango previsto."
  ```

### 7. Reflexionar
- Actualizar la memoria semántica: refinar la regla sobre efectividad del pre‑enfriamiento.
- Refinar políticas procedurales: ajustar el umbral de activación automática.
- Reportar al orquestador (N3) las métricas de ahorro y precisión de hipótesis.

## Integración con el ecosistema hospitalario

- **Con el clinical-agent (N2):** coordinación para no afectar condiciones clínicas. El energy-agent solicita confirmación antes de modificar la climatización de cualquier zona.
- **Con el hospital-orchestrator (N3):** reporta métricas de consumo (kWh/cama/año) y ahorro respecto a la línea base. El orquestador ajusta el `trust_score` en el `registry.yaml`.
- **Con el gemelo digital (Tejido T3):** el estado térmico del hospital y el nivel de las baterías se reflejan en el gemelo digital para simulaciones "what‑if".

## Ejemplo de uso

```
Agente energético (N2, fase Razonar):
  "Predicción de demanda para mañana: pico de 320 kW a las 14:00.
   Ocupación: consultas externas bajan a las 14:00. Temperatura exterior: 35°C."

Agente energético (fase Hipótesis):
  "H018-2026-06-07: pre‑enfriar consultas externas (22→20°C) entre 13:00-14:00.
   Efecto previsto: reducción de 45 kW (14% de ahorro). Confianza: 0.89."

Agente energético (fase Verificar):
  "Validación neuro-simbólica: zonas críticas no afectadas.
   SOC mínimo de baterías garantizado. Autorizado."

Agente energético (fase Ejecutar):
  "Consultando al clinical-agent: ¿restricciones en consultas externas?
   Clinical-agent: sin restricciones. Procediendo con pre‑enfriamiento."

Agente energético (fase Evaluar):
  "Resultado: ahorro real 42 kW (error 6.7%). Hipótesis H018 validada.
   Archivando en validated/."

Agente energético (fase Reflexionar):
  "Actualizando regla semántica: confianza incrementada de 0.89 a 0.91."
```
