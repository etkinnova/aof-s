# Changelog

Todos los cambios notables de AOF‑S serán documentados en este archivo.

El formato está basado en [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
y este proyecto adhiere a [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [1.0.0] — 2026-06-07

### Added
- **Modelo ASA v1.0** (`spec/asa.yaml`): definición formal de los tres niveles de agencia (Reactivo, Sistémico, Orquestado) con ciclo operativo de 7 fases, memoria cuádruple, meta‑cognición y razonamiento neuro‑simbólico híbrido.
- **Estándar AOF‑S v1.0** (`spec/aof-s.yaml`): especificación ejecutable del estándar de archivos versionables, con carga progresiva de contexto, principios de diseño y matriz de contexto por fase.
- **Noosfile v1alpha2** (`spec/noosfile.yaml`): manifiesto declarativo de orquestación con secciones de seguridad Zero Trust, evaluación e inferencia Local‑First.
- **NSL v1.0** (`spec/nsl.yaml`): Capa Semántica Noosistémica con Semantic Handshake, Trust Score programático, contratos semánticos aprendidos y bridge OSI.
- **Grafo Causal Probabilístico** (`spec/causal_graph.proto`): esquema protobuf para modelar P(Y|do(X)) con distribuciones de probabilidad actualizables por reflexión.
- **README.md**: presentación del estándar con el triángulo del Noosistema, niveles de agencia, principios de diseño y guía de inicio rápido.
- **Documentación (`docs/`)**:
  - `why.md`: artículo filosófico y estratégico.
  - `how.md`: guía práctica de adopción.
  - `guides/quickstart.md`: guía de inicio en 5 minutos.
- **Plantillas (`templates/`)**:
  - `ultra-light/`: Nivel 1 reducido (2 archivos).
  - `reactive/`: Nivel 1 completo (9 archivos).
  - `systemic/`: Nivel 2 con memoria y modelo del mundo (16 archivos).
  - `orchestrated/`: Nivel 3 con `ASA.yaml`, `causal_graph.proto`, `registry.yaml` y memoria cuádruple (30+ archivos).
- **Licencia Apache 2.0** (`LICENSE`).