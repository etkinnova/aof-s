---
skill_name: rag_skill
version: "1.0.0"
sector: "e-commerce"
description: >
  Habilidad canónica para que el agente orquestador indexe, consulte y mantenga
  actualizado un corpus de conocimiento experto usando una base de datos vectorial
  local (LanceDB). Permite al agente recuperar contexto relevante y aumentar sus
  prompts con conocimiento de dominio antes de razonar o actuar.
triggers:
  - "Necesito consultar mi corpus experto sobre X"
  - "Actualiza mi base de conocimiento con los últimos documentos"
  - "¿Qué sabemos sobre Y según nuestra documentación interna?"
  - "Indexa los nuevos documentos en el corpus"
dependencies:
  vector_store: "lancedb"
  embedding_model: "text-embedding-3-small"
  expert_corpus: "./04_knowledge/expert_corpus/"
tools_required:
  - read_file
  - write_to_file
  - execute_command
---

# RAG Skill: Recuperación Aumentada de Conocimiento Experto

## Propósito

Dotar al agente orquestador de la capacidad de:
- Indexar documentos de `expert_corpus/source_documents/` en la base vectorial `expert_corpus/vector_store/`.
- Realizar búsquedas semánticas sobre el corpus para recuperar fragmentos relevantes.
- Aumentar prompts con el conocimiento recuperado antes de razonar o actuar.
- Mantener el corpus actualizado desde fuentes externas configuradas.

## Flujo de indexación (`index_corpus`)

Cuando el agente detecta nuevos documentos o recibe la instrucción:

1. **Escanear `source_documents/`:**
   - Listar todos los archivos `.md`, `.pdf`, `.txt`, `.yaml` en el directorio y subdirectorios.
   - Comparar con el archivo de estado `vector_store/index_state.json` para detectar cambios (nuevos, modificados, eliminados).

2. **Procesar documentos:**
   - Para cada documento nuevo o modificado:
     - Dividir en chunks de ~1000 tokens con solapamiento de 100 tokens.
     - Para cada chunk, generar embedding usando el modelo `text-embedding-3-small`.
     - Almacenar en LanceDB con metadata: `source_file`, `chunk_id`, `timestamp`, `tags`.

3. **Actualizar estado:**
   - Escribir `vector_store/index_state.json` con el nuevo estado del índice.

4. **Registrar en memoria:**
   - Añadir entrada en `observations/feedback_log.yaml`: `{event: "corpus_indexed", files_added: N, chunks_total: M, timestamp: ISO}`.

## Flujo de consulta (`retrieve`)

Cuando el agente necesita conocimiento experto:

1. **Recibir query** del usuario, de otro agente o del propio flujo de razonamiento.
2. **Reformular query** si es necesario para optimizar la búsqueda (opcional, usar LLM ligero).
3. **Ejecutar búsqueda semántica:**
   - Embedding de la query.
   - Búsqueda top-K (K=5 por defecto, ajustable) en LanceDB.
   - Recuperar chunks + metadata.
4. **Re-rankeo opcional** con modelo de reranking si la precisión es crítica.
5. **Devolver contexto** formateado para inclusión en prompt:

   ```
   [CONTEXTO RECUPERADO DE MI CORPUS EXPERTO]
   Fuente: {source_file}
   Relevancia: {score}
   ---
   {chunk_text}
   ---
   ```

## Flujo de actualización (`update_knowledge`)

Para mantener el corpus al día:

1. **Fuentes externas configuradas** en `expert_corpus/sources.yaml`:
   ```yaml
   sources:
     - type: "web_scrape"
       url: "https://docs.example.com/api-reference"
       frequency: "weekly"
     - type: "git_repo"
       repo: "git@github.com:org/internal-docs.git"
       path: "/architecture/"
       frequency: "daily"
     - type: "api_pull"
       endpoint: "https://internal-api.example.com/knowledge-graph"
       frequency: "hourly"
   ```
2. **Ejecutar recolección** según frecuencia.
3. **Pipeline de ingesta**: descargar → convertir a texto → guardar en `source_documents/` → disparar `index_corpus`.
4. **Validar calidad**: verificar que los chunks recuperados son coherentes (opcional, usar LLM para evaluar una muestra).

## Integración con el ciclo ASA

Durante las fases de **Razonar** y **Reflexionar**, el agente intercala llamadas a `retrieve` para enriquecer su contexto:

1. **Razonar (Fase 2):** Ante una misión nueva, consultar: "¿Qué intervenciones previas relacionadas hay en mi corpus?"
2. **Hipótesis (Fase 3):** Consultar: "¿Qué patrones de fallo se han documentado para esta acción?"
3. **Reflexionar (Fase 7):** Si el resultado diverge de la hipótesis, indexar la nueva lección en el corpus: "Este resultado no predicho se documenta como nuevo caso."

## Ejemplo de uso

```
Agente orquestador (N3, fase Razonar):
  "Necesito consultar mi corpus experto sobre PCI DSS antes de esta intervención."

  retrieve("PCI DSS requirements for database provisioning")
  → Recupera 3 chunks de source_documents/pci_compliance.md
  → Incorpora al prompt: "Según PCI DSS 4.0, las bases de datos de pago deben
     tener cifrado en reposo, TLS 1.3 en tránsito, y auditoría de accesos habilitada."

Agente orquestador (al agente db-provisioner):
  "De acuerdo. Además de la creación estándar, aplica las configuraciones
   específicas para PCI DSS que tengo documentadas: cifrado en reposo (AES-256),
   TLS 1.3 forzado, y logging de auditoría completo."
```

## Mantenimiento de la habilidad

- El índice debe reconstruirse completamente si cambia el modelo de embedding.
- `index_state.json` debe versionarse para auditoría.
- Los chunks huérfanos (de documentos eliminados) deben limpiarse periódicamente.
