# testing_skill

<!--
  Habilidad modular para ejecutar tests.
  El agente carga esta skill cuando la tarea requiere validación.
-->

## Propósito
Ejecutar la suite de tests y verificar que la cobertura supera el umbral mínimo.

## Comando
```bash
pnpm test -- --coverage
```

## Criterio de éxito
- Todos los tests pasan.
- Cobertura >= 80%.