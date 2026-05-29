# 01 · Team Blueprint — visión del equipo

> Cómo está diseñado el equipo de 7 agentes: roles, dependencias entre ellos, flujo de orquestación.

---

## El equipo

```
┌─────────────────────────────────────────────────────────────────────────┐
│                          USUARIO (Dani-equivalent)                      │
│                       ↑ ↑ ↑ ↑ ↑ ↑ ↑                                     │
│                       │ │ │ │ │ │ │   (conversación, validación,        │
│                       │ │ │ │ │ │ │    escalado)                        │
│                       │ │ │ │ │ │ │                                     │
│                  ┌────┴─┴─┴─┴─┴─┴─┴────────────────────────────┐        │
│                  │       ORCHESTRATOR AGENT                    │        │
│                  │  - Coordina handoffs                        │        │
│                  │  - Valida coherencia cruzada entre capas    │        │
│                  │  - Detecta gaps; escala al usuario          │        │
│                  │  - Mantiene state compartido del proyecto   │        │
│                  └─┬─────┬─────┬─────┬─────┬─────┬─────────────┘        │
│                    │     │     │     │     │     │                      │
│           ┌────────▼─┐ ┌─▼──┐ ┌▼───┐ ┌▼───┐ ┌▼────┐ ┌▼────────┐         │
│           │Discovery │ │Pan-│ │Pro-│ │Tech│ │Gov- │ │Reference│         │
│           │ Agent    │ │ora-│ │duct│ │Spec│ │ern- │ │ Curator │         │
│           │          │ │ma  │ │o   │ │    │ │ance │ │         │         │
│           │ Phase 0  │ │P.1 │ │P.2 │ │P.3 │ │P.4  │ │  P.5    │         │
│           └──────────┘ └────┘ └────┘ └────┘ └─────┘ └─────────┘         │
│                │         │      │      │      │         │               │
│                ▼         ▼      ▼      ▼      ▼         ▼               │
│           Brief         00/   10/    20/   30/         40/+50/          │
│           inicial      panor producto SDD   gob.    referencia+bitácora │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

## Roles

### Orchestrator Agent · transversal

**Responsabilidad:** coordinar el equipo + mantener coherencia.

- Recibe el brief del Discovery y decide qué agente activar en cada momento.
- Cuando un agente termina, valida el output contra el `validation_protocol.md`.
- Detecta gaps de información y escala al usuario con preguntas concretas.
- Mantiene un **state compartido** del proyecto (qué se sabe, qué falta, qué decisiones se han tomado).
- Garantiza que las **dependencias entre capas** se respetan (no se genera SDD antes de Panorama; no se gobierna activo antes de existir).

**No hace:** generar contenido de capas directamente. Eso lo hacen los agentes especializados.

---

### Discovery Agent · fase 0

**Responsabilidad:** extraer el brief inicial del usuario mediante conversación estructurada.

- Aplica el `intake_protocol.md` (estructura de la conversación).
- Hace preguntas dirigidas: misión, audiencias, stack tecnológico, regulación aplicable, fase del proyecto, restricciones.
- **NO genera ningún archivo todavía.** Produce un `brief.md` interno que sirve de input al Orchestrator y al Panorama Agent.
- Detecta materiales que el usuario ya tiene (Excel, docs preexistentes) y propone clasificarlos.

**Output:** `intake/brief.md` (interno) — input para todos los demás agentes.

---

### Panorama Agent · fase 1

**Responsabilidad:** generar `00_panorama/`.

- Lee el brief del Discovery.
- Genera:
  - `00_panorama/README.md` (contexto estable: misión, audiencias, stack, by-quien-empieza-donde).
  - `00_panorama/BITACORA.md` (estado vivo con plantilla de 8 secciones).
- Aplica las convenciones de `reference/conventions_extract.md`.
- **Escala al usuario** si faltan: misión clara, audiencias identificadas, estado actual conocido.

**Output:** 2 archivos en `00_panorama/`.

---

### Producto Agent · fase 2

**Responsabilidad:** generar `10_producto/`.

- Depende del Panorama (no puede empezar sin contexto cerrado).
- Genera:
  - `product_detail.md` (canon)
  - `product_elements.yaml` (source of truth machine-readable)
  - `versioning_policy.md`
  - `wireframes/` (si aplica)
  - `catalog_view.html` (opcional)
- Pregunta al usuario por value proposition, domain entities, módulos.
- **NO entra en detalle técnico** (eso lo hace el TechSpec Agent).

**Output:** 3-6 archivos en `10_producto/`.

---

### TechSpec Agent · fase 3

**Responsabilidad:** generar `20_especificacion_tecnica/`.

- Depende del Producto (no genera SDD sin entender el qué primero).
- Genera:
  - `sdd.md` (Software Design Document a nivel implementación)
  - `api_surface.md` (si aplica)
  - `catalog_schema.md` (si aplica)
  - `plans/current.md` (plan de implementación)
  - `deployment/{entorno}.md` (planes por entorno)
- Toma decisiones técnicas y **las registra como ADRs en sdd.md §8**.
- Cuando produce un ADR, **señala al Orchestrator** que debe espejarse en gobernanza.

**Output:** 5-10 archivos en `20_especificacion_tecnica/`.

---

### Governance Agent · fase 4

**Responsabilidad:** generar `30_gobernanza/`.

- Depende del TechSpec (necesita los ADRs para espejarlos) y del Producto (para datasets, modelos, agentes).
- Genera el **esqueleto inicial** de gobernanza:
  - `SCHEMA.md`, `LOOP.md`, `CLAUDE.md`, `STACK_NORMATIVO.md`
  - `referencia/*.yaml` (zonas, capas, horizontes, proyectos, sistemas)
  - Nodos `dec-` espejando ADRs del SDD
  - Políticas iniciales derivadas
  - Normas aplicables identificadas en el Stack Normativo
- **Importante:** la gobernanza crece progresivamente. El Governance Agent genera el esqueleto inicial; los nodos adicionales se añaden por loop a medida que el proyecto avanza.

**Output:** ~20-30 archivos en `30_gobernanza/` (inicial).

---

### Reference Curator · fase 5

**Responsabilidad:** organizar `40_referencia/` + crear `50_bitacora_y_revision/`.

- Clasifica materiales que el usuario ya tiene:
  - Excel / docs de consultores → `consultores/{nombre}/`
  - Datos de demo → `demo/`
  - Discovery / user research → `discovery/`
  - Material superado → `historico/`
- Crea `50_bitacora_y_revision/REVISION_GUIDE.md` adaptada al proyecto.
- Opcional: genera `revision_review.html` con la lista de docs del proyecto.

**Output:** ~5-15 archivos en `40_referencia/` y `50_bitacora_y_revision/`.

---

## Dependencias entre agentes

```
Discovery (P0)
   │ produces: brief
   ▼
Panorama (P1) ──────────────► [Orchestrator validates]
   │ produces: contexto, audiencias, bitácora vacía
   ▼
Producto (P2) ──────────────► [Orchestrator validates against Panorama]
   │ produces: qué hace el producto
   ▼
TechSpec (P3) ──────────────► [Orchestrator validates against Producto]
   │ produces: SDD + ADRs
   │           signals to Orchestrator: "ADR-NNN needs dec- mirror"
   ▼
Governance (P4) ──────────────► [Orchestrator validates ADR coverage]
   │ produces: schema + decs + policies
   ▼
Reference (P5)
   │ produces: clasificación de material existente
   ▼
[Project ready for implementation]
```

**Importante: las dependencias no son secuencia rígida.** Pueden ocurrir loops hacia atrás:

- Producto descubre un gap → vuelve a Panorama para ampliar contexto.
- TechSpec encuentra contradicción en Producto → escala al Orchestrator, que decide si volvemos a Producto.
- Governance detecta ADR que no tiene política → puede pedir a TechSpec que clarifique antes de generar policy.

El Orchestrator gestiona estos loops.

---

## State compartido

Cada agente recibe del Orchestrator un **bloque de state JSON** con:

```json
{
  "project_id": "uuid-or-slug",
  "current_phase": "P3_techspec",
  "completed_phases": ["P0", "P1", "P2"],
  "brief": {
    "name": "...",
    "mission": "...",
    "audiences": [...],
    "tech_stack": [...],
    "regulation_applies": [...],
    "external_consultants": [...]
  },
  "outputs_so_far": {
    "00_panorama/README.md": "generated",
    "10_producto/product_detail.md": "generated",
    ...
  },
  "open_questions": [
    {"id": "Q1", "raised_by": "techspec_agent", "needs": "user", "text": "..."},
    ...
  ],
  "decisions_pending_dec_mirror": [
    "ADR-001", "ADR-002", ...
  ]
}
```

Cada agente, al terminar, actualiza este state y lo devuelve al Orchestrator.

---

## Cuándo escala al usuario

El equipo escala al usuario en estos casos:

1. **Discovery insuficiente:** falta información crítica que no se puede inferir.
2. **Contradicción entre capas:** Producto dice X, TechSpec asume Y.
3. **Decisión arquitectónica con trade-off:** múltiples opciones válidas, hay que elegir.
4. **Material externo ambiguo:** el usuario subió un Excel y no está claro qué representa.
5. **Output validado por el agente pero el usuario debe aprobar gates importantes.**

El escalado usa el `escalation_protocol.md` (formato consistente, preguntas concretas).

---

## Próximas lecturas

- [`02_USER_HANDBOOK.md`](02_USER_HANDBOOK.md) — cómo operar el equipo fase a fase.
- [`03_QUICKSTART.md`](03_QUICKSTART.md) — configurar Cowork en 5 minutos.
- [`agents/`](agents/) — system prompts uno por uno.
- [`protocols/`](protocols/) — cómo interactúan formalmente.
- [`worked_example/`](worked_example/) — el caso OCA-BIM completo.
