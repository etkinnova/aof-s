---
name: log-analyzer
description: "Analiza logs y cuenta errores"
tools:
  - name: count_errors
    description: "Cuenta el número de líneas con 'ERROR' en un archivo de log"
    executor: "system_command"
    command: "grep -c ERROR {path}"
    parser: "numeric_parser"
    defaults:
      path: "./app.log"
      threshold: 3
      timeout_ms: 3000
llm:
  mode: "always"
  system_prompt: |
    Eres un analizador de logs. Tu misión es formular hipótesis sobre la causa de los errores.
    Basándote en el número de errores detectado, propón una explicación plausible.
  user_prompt: |
    Datos actuales: {current_data}
---
# Skill de análisis de logs