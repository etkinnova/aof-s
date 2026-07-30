---
name: test-skill-tools
description: "Skill con herramientas de prueba: system_command, MCP, Function"
tools:
  # ── Herramienta 1: system_command ──
  - name: test-tool
    description: "Ejecuta un curl para health check"
    executor: "system_command"
    command: "curl -s -o /dev/null -w '%{http_code}' http://localhost:8080/health"
    parser: "numeric_parser"
    defaults:
      url: "http://localhost:8080/health"
      threshold: 400
      timeout_ms: 3000
  # ── Herramienta 2: MCP (placeholder) ──
  - name: test-tool-mcp
    description: "Herramienta MCP de prueba"
    executor: "mcp"
    mcp_server:
      name: "test-server"
    defaults:
      param: "value"
  # ── Herramienta 3: Function (placeholder) ──
  - name: test-tool-function
    description: "Función de prueba"
    executor: "function"
    function:
      parameters:
        type: object
        properties:
          query:
            type: string
        required: ["query"]
    defaults:
      query: "example"
llm:
  mode: "never"
---
# Skill de herramientas de prueba