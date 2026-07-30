# Pruebas de ejecución de herramientas en `asa_agent`

Este directorio contiene un único `.aof/` que permite probar los cuatro casos de uso de ejecución de herramientas:
1. `system_command` (health check real)
2. MCP (placeholder)
3. Function (placeholder)
4. Skill sin herramientas (solo `body` Markdown)

---
### 📂 Estructura del directorio

```
test_tools/
├── .aof/
│   ├── 00_manifest.yaml
│   ├── 01_context_static.md
│   ├── 02_agent_spec.yaml
│   ├── 03_mission.yaml
│   ├── 05_skills/
│   │   ├── test-skill-tools/
│   │   │   └── SKILL.md          # Contiene herramientas: system_command, MCP, Function
│   │   └── test-skill-body/
│   │       └── SKILL.md          # Sin herramientas, solo instrucciones (body)
│   └── 09_observations/          # Se genera automáticamente
└── README.md                      # Instrucciones para ejecutar cada prueba
```
## 📋 Requisitos

- Runtime `asa_agent` compilado (`cargo build --workspace`).
- (Opcional) Ollama en ejecución para la prueba 4 (`ollama serve`).

---

## 🚀 Cómo ejecutar las pruebas

### 1. Copiar el directorio `.aof/` a la raíz del proyecto

```bash
# Desde la raíz del proyecto
cp -r tests/test_tools/.aof ./
```

Alternativamente, puedes usar `AOF_PATH` para apuntar a este directorio sin copiarlo:

```bash
$env:AOF_PATH = "./tests/test_tools/.aof"
```

### 2. Configurar la prueba deseada

Edita los archivos `.aof/00_manifest.yaml` y `.aof/02_agent_spec.yaml` según la prueba que quieras realizar.

---

## 🧪 Pruebas disponibles

### Prueba 1 – `system_command` (Health Check)

**Configuración:**
- `00_manifest.yaml`: `active_skills: [test-skill-tools]`
- `02_agent_spec.yaml`: `allowed_tools: [test-tool]` (descomenta la línea)

**Ejecución:**
```bash
$env:RUST_LOG="debug"
cargo run -p asa-n1
```

**Resultado esperado:**
- El agente ejecuta `curl` a `http://localhost:8080/health`.
- Si el endpoint no existe, el comando falla y se registra en `feedback.yaml`.
- `feedback.yaml` contendrá el código de respuesta o un error.

---

### Prueba 2 – MCP (placeholder)

**Configuración:**
- `00_manifest.yaml`: `active_skills: [test-skill-tools]`
- `02_agent_spec.yaml`: `allowed_tools: [test-tool-mcp]` (descomenta la línea)

**Ejecución:**
```bash
$env:RUST_LOG="debug"
cargo run -p asa-n1
```

**Resultado esperado:**
- El log mostrará: `[execute] Herramienta 'test-tool-mcp' es de tipo MCP... La ejecución MCP no está implementada aún.`
- `feedback.yaml`: `- test-tool-mcp: MCP no implementado (servidor: Some("test-server"))`

---

### Prueba 3 – Function (placeholder)

**Configuración:**
- `00_manifest.yaml`: `active_skills: [test-skill-tools]`
- `02_agent_spec.yaml`: `allowed_tools: [test-tool-function]` (descomenta la línea)

**Ejecución:**
```bash
$env:RUST_LOG="debug"
cargo run -p asa-n1
```

**Resultado esperado:**
- El log mostrará: `[execute] Herramienta 'test-tool-function' es de tipo Function... Las funciones se delegan al LLM.`
- `feedback.yaml`: `- test-tool-function: Function (delegado al LLM)`

---

### Prueba 4 – Skill sin herramientas (solo `body` Markdown)

**Configuración:**
- `00_manifest.yaml`: `active_skills: [test-skill-body]` (comenta `test-skill-tools` y descomenta `test-skill-body`)
- `02_agent_spec.yaml`: `allowed_tools: []` (vacío, no hay herramientas)

**Ejecución:**
```bash
$env:RUST_LOG="debug"
cargo run -p asa-n1
```

**Resultado esperado:**
- El log mostrará: `[reason] Skill 'test-skill-body' sin herramientas; se ha inyectado su cuerpo Markdown en el prompt.`
- El agente consulta al LLM (porque `llm.mode: always`).
- La respuesta del LLM debe contener el número 42 (o una referencia a él).

---

## 📤 Verificación de resultados

Los resultados se registran en `.aof/09_observations/feedback.yaml`. Puedes revisarlo después de cada ejecución:

```bash
cat .aof/09_observations/feedback.yaml
```

---

## 🧹 Limpieza

Para volver a ejecutar una prueba, puedes eliminar el archivo `feedback.yaml` o sobrescribirlo con una nueva ejecución:

```bash
rm .aof/09_observations/feedback.yaml
```

---

## 📦 ¿Qué hacer después de las pruebas?

Si deseas conservar estos artefactos como paquetes reutilizables, considera copiarlos a `~/.noosystem/packages/` y añadir un README específico para cada caso. Este directorio es un punto de partida.

---

¡Disfruta probando `asa_agent`!
```

---

## ✅ Uso práctico

1. Copia el directorio `tests/all-tests/` a tu proyecto.
2. Sigue las instrucciones del README para ejecutar cada prueba.
3. Si encuentras algún error, comparte la salida para depurar.

Este enfoque mantiene todas las pruebas en un solo lugar, simplifica la gestión y permite probar los cuatro casos sin necesidad de tener múltiples directorios. ¿Te parece adecuado?