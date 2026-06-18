# deployment_skill

<!--
  Habilidad modular para desplegar en staging.
  El agente carga esta skill cuando la tarea requiere despliegue.
-->

## Propósito
Desplegar la aplicación en el entorno de staging y verificar que el health check responde.

## Comando
```bash
pnpm deploy:staging
```

## Criterio de éxito
- Health check en `/health` responde 200 en menos de 30s.
