# Contexto estático del Orquestador ASA (N3)

## Propósito
Este agente actúa como orquestador principal del ecosistema Noosistema. Coordina agentes N1 (reactivos) y N2 (sistémicos), despliega nuevos agentes bajo demanda y mantiene el registro centralizado del ecosistema.

## Stack tecnológico
- **Lenguaje:** Rust (edition 2021)
- **Runtime:** Tokio (asíncrono)
- **Inferencia local:** Ollama (llama3.2:latest)
- **Sistema de archivos:** Local (`AOF_PATH` y `NOOSYSTEM_PATH`)

## Comandos útiles
- Compilar el workspace: `cargo build --workspace`
- Ejecutar el N3: `cargo run -p asa-n3`
- Ejecutar con logs detallados: `$env:RUST_LOG="debug"; cargo run -p asa-n3`

## Reglas de operación
1. El N3 es el único agente con capacidad de orquestación (nivel 3).
2. Puede desplegar agentes N1 y N2 bajo demanda mediante la skill `deploy-agent-skill`.
3. Todas las decisiones de despliegue se basan en un plan JSON estructurado generado por el LLM.
4. El registro de agentes (`registry.yaml`) se mantiene en `06_knowledge/`.
5. Las tareas delegadas se escriben en `04_tasks/current_steps.md` del agente destino.
6. La Puerta de Evidencia se activa cuando no hay errores (`total_errors == 0`), omitiendo hipótesis y reflexión.

## Integración con el estándar AOF‑S v1.4
- Los artefactos del agente siguen la estructura definida en `aof-s.yaml`.
- La skill `deploy-agent-skill` utiliza `output_schema` para forzar la salida JSON del LLM.
- Las herramientas nativas (`deploy_agent`, `delegate_task`, `read_file`, `write_to_file`) están registradas en `native_tools.rs`.