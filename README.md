# 5-Layer Docs Cowork Team

> **Pack de configuración para Claude Cowork** que materializa un equipo de 7 agentes especializados en generar la documentación de un proyecto nuevo siguiendo el **Framework de 5 Capas** de forma iterativa, asistida por conversación con el usuario.
>
> **Versión:** 0.1 · **Idioma:** prompts en inglés, guías en español · **Origen:** caso OCA-BIM™ (mayo 2026)

---

## Qué es esto

Es la documentación operativa que necesitas para configurar un **equipo de agentes en Claude Cowork** (o equivalente) que sepa:

1. **Conversar contigo** para entender un proyecto nuevo (Discovery).
2. **Generar la documentación de las 5 capas** del framework:
   - `00_panorama/` — contexto + bitácora
   - `10_producto/` — qué hace el producto
   - `20_especificacion_tecnica/` — SDD + planes técnicos
   - `30_gobernanza/` — gobernanza como código
   - `40_referencia/` + `50_bitacora_y_revision/` — material de soporte
3. **Iterar contigo** en cada capa, validar coherencia entre ellas y escalar dudas explícitamente.

El equipo está diseñado para **NO improvisar** y **NO saltar fases**. La filosofía es la misma que en el proyecto original: las decisiones se registran antes de generar artefactos, las dependencias entre capas se respetan, y el usuario es siempre el árbitro final.

---

## Cómo navegarlo

| Si eres... | Empieza por |
|---|---|
| **Usuario que configura Cowork por primera vez** | [`03_QUICKSTART.md`](03_QUICKSTART.md) |
| **Quieres entender el equipo antes de configurarlo** | [`01_TEAM_BLUEPRINT.md`](01_TEAM_BLUEPRINT.md) |
| **Vas a operar el equipo con tu proyecto** | [`02_USER_HANDBOOK.md`](02_USER_HANDBOOK.md) |
| **Buscas el prompt de un agente concreto** | [`agents/`](agents/) |
| **Quieres ver cómo se ve cuando está bien** | [`worked_example/`](worked_example/) |

---

## Estructura del pack

```
5-layer-docs-cowork/
├── README.md                        Este archivo
├── 01_TEAM_BLUEPRINT.md             Visión del equipo: roles, dependencias, flujo
├── 02_USER_HANDBOOK.md              Cómo el usuario opera el equipo fase a fase
├── 03_QUICKSTART.md                 Guía de 5 minutos para configurar Cowork
│
├── agents/                          System prompts (uno por agente, en inglés)
│   ├── _common_context.md           Knowledge base compartida por todos
│   ├── discovery_agent.md
│   ├── panorama_agent.md
│   ├── producto_agent.md
│   ├── techspec_agent.md
│   ├── governance_agent.md
│   ├── reference_agent.md
│   └── orchestrator_agent.md
│
├── protocols/                       Cómo el equipo interactúa
│   ├── intake_protocol.md           Estructura de la conversación inicial
│   ├── handoff_protocol.md          Cómo un agente entrega al siguiente
│   ├── validation_protocol.md       Checks cruzados entre capas
│   ├── iteration_protocol.md        Cómo iterar (no es waterfall)
│   └── escalation_protocol.md       Cuándo parar y preguntar al usuario
│
├── deliverable_specs/               Qué debe producir cada agente
│   ├── 00_panorama_spec.md
│   ├── 10_producto_spec.md
│   ├── 20_techspec_spec.md
│   ├── 30_governance_spec.md
│   ├── 40_referencia_spec.md
│   └── 50_bitacora_spec.md
│
├── worked_example/                  Caso OCA-BIM como referencia
│   ├── README.md
│   ├── intake_session.md            Transcripción del discovery
│   ├── iterations_log.md            Iteraciones que ocurrieron
│   └── final_outputs_map.md         Mapa de qué generó cada agente
│
└── reference/                       Contexto compartido
    ├── framework_5_capas.md         Resumen del framework
    ├── conventions_extract.md       Naming, versionado, idioma
    ├── templates_index.md           Punteros a plantillas
    └── known_antipatterns.md        Qué el equipo NO debe hacer
```

---

## Pre-requisitos

- Cuenta en **Claude Cowork** (o herramienta equivalente que permita configurar múltiples agentes con system prompts).
- Acceso a **Claude Sonnet 4.6 o superior** como modelo base (el equipo está calibrado para razonamiento de ese nivel).
- **Tiempo:** ~30 min de setup inicial + iteración con el equipo durante el proyecto.

---

## Filosofía

1. **El equipo no improvisa.** Cada agente tiene scope explícito; si algo cae fuera, escala.
2. **El flujo no es waterfall.** Itera entre capas si la información cambia.
3. **El usuario es siempre el árbitro final.** Los gates de validación son humanos.
4. **La gobernanza crece progresivamente.** No se genera entera el día 1.
5. **Las improvisaciones invisibles son lo que el equipo está diseñado para impedir.**

---

## Origen y licencia

Este pack es la destilación operativa del trabajo realizado en el proyecto **OCA-BIM™** (TechBizDesign + AgileFOCUS, mayo 2026) donde se diseñó, refinó y aplicó por primera vez el Framework de 5 Capas. El caso OCA-BIM vive en [`worked_example/`](worked_example/) como referencia.

**Uso interno y replicable.** Adapta libremente a tus proyectos.

---

*Última actualización: 2026-05-29 · Mantenedor: Dani Cardelús*
