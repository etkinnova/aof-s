# Quickstart: Tu primer agente AOF‑S en 5 minutos

**Objetivo:** Tener un agente funcional con el mínimo esfuerzo posible, usando el modo ultra‑light del Nivel 1 (Reactivo).

---

## 1. Inicializa tu agente

Abre una terminal en la raíz de tu proyecto y ejecuta:

```bash
noctl init --level 1 --ultra-light
```

Esto crea una carpeta `.aof/` con solo dos archivos:

```
.aof/
├── 00_manifest.yaml
└── 01_context_static.md
```

---

## 2. Personaliza tu contexto

Edita `.aof/01_context_static.md` con la información de tu proyecto. Sé breve; el agente solo necesita lo esencial:

```markdown
# Stack
Node.js 22, PostgreSQL 16, pnpm

# Comandos
- build: `pnpm build`
- test: `pnpm test -- --coverage`

# Reglas
1. No modificar `schema.sql` sin PR aprobado.
```

---

## 3. Usa tu agente

Cualquier LLM que lea el directorio `.aof/` puede operar como un agente AOF‑S. El archivo `00_manifest.yaml` le indica qué estándar está usando.

**Ejemplo de interacción:**

> *"Agente, revisa el archivo `src/api/orders.ts` y sugiere mejoras de rendimiento."*

El agente usará `01_context_static.md` para entender tu stack y tus reglas antes de responder.

---

## 4. Verifica que funciona

Revisa `00_manifest.yaml`. Debe reflejar tu proyecto:

```yaml
framework: AOF‑S v1.0
level: 1
project: "mi-api"
created: "2026-06-07"
agent_instance: "agent-001"
```

---

## 5. ¿Quieres más? Escala

Cuando necesites que tu agente **recuerde** entre sesiones y **aprenda** de sus intervenciones:

```bash
noctl upgrade --level 2
```

Cuando necesites **orquestar múltiples agentes**:

```bash
noctl upgrade --level 3
noctl apply -f noosfile.yaml
```

---

## ¿Qué sigue?

- **Guía completa de adopción:** [`docs/how.md`](../how.md)
- **Filosofía y motivación:** [`docs/why.md`](../why.md)
- **Especificación formal:** [`spec/aof-s.yaml`](../../spec/aof-s.yaml)