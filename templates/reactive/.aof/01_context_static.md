# Contexto estático — Nivel 1 (Reactivo)

<!--
  Información inmutable del proyecto. El agente la lee en cada sesión
  para entender tu stack, comandos y reglas.
  Sé breve: solo lo esencial. El agente infiere el resto del código.
-->

## Stack
Node.js 22, PostgreSQL 16, pnpm

## Comandos
- build: `pnpm build`
- test: `pnpm test -- --coverage`
- lint: `pnpm lint`

## Reglas
1. No modificar `schema.sql` sin PR aprobado.
2. Usar tipos generados desde OpenAPI.
3. Commits semánticos (conventional commits).

## Documentación indexada
| Recurso | Ruta |
|----------|------|
| API docs | docs/api/README.md |
| Esquema DB | docs/db/schema.md |