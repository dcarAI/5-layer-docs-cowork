# 02 · User Handbook — cómo operar el equipo fase a fase

> Manual del usuario para usar el equipo de Cowork con un proyecto nuevo. Asume que ya tienes Cowork configurado siguiendo [`03_QUICKSTART.md`](03_QUICKSTART.md).

---

## Flujo general

```
Sesión 1   →   Discovery (P0)                    ~30-60 min de conversación
Sesión 2   →   Panorama (P1)                     ~15-30 min
Sesión 3   →   Producto (P2)                     ~30-60 min
Sesión 4   →   TechSpec (P3)                     ~60-120 min — es la más densa
Sesión 5   →   Governance (P4)                   ~30-60 min
Sesión 6   →   Reference (P5)                    ~15-30 min
Sesión 7+  →   Iteraciones según necesidad       variable
```

Cada sesión puede ser una sola conversación larga o varias cortas. El Orchestrator mantiene state entre sesiones (si tu Cowork preserva contexto entre conversaciones; si no, guardas el state manualmente).

---

## Fase 0 · Discovery

**Lanza:** "Hola Discovery Agent. Voy a contarte sobre un proyecto nuevo."

**Qué pasa:**
- El Discovery Agent te hará entre 12 y 20 preguntas estructuradas.
- Te preguntará por audiencias, misión, stack, regulación, materiales que ya tienes, fase del proyecto, restricciones.
- **NO te interrumpas.** Responde una pregunta a la vez. Si no sabes algo, dilo: "no lo sé", "lo decido después".
- Al final, te dará un resumen del brief para que valides.

**Output:** `intake/brief.md` (el agente te lo muestra; lo guardas como referencia).

**Qué hacer después:**
1. Lee el brief generado.
2. Si hay algo mal, corriges en la misma conversación.
3. Cuando lo apruebas, le dices al Orchestrator: "Brief aprobado, pasa a Fase 1".

---

## Fase 1 · Panorama

**Lanza:** "Orchestrator, pasa a Fase 1. Activa Panorama Agent."

**Qué pasa:**
- Panorama Agent lee el brief y genera:
  - `00_panorama/README.md` (con tu misión, audiencias, stack, by-quien-empieza-donde).
  - `00_panorama/BITACORA.md` (esqueleto con 8 secciones, fechado hoy).
- Te muestra los archivos generados como bloques de texto.
- Te puede preguntar 2-3 cosas para completar gaps.

**Qué hacer después:**
1. Lee los dos archivos.
2. Si falta o sobra algo, le dices y los regenera.
3. Cuando los apruebas, los **guardas en tu repo del nuevo proyecto** en `docs/00_panorama/`.
4. Le dices al Orchestrator: "Panorama aprobado y guardado, pasa a Fase 2".

---

## Fase 2 · Producto

**Lanza:** "Orchestrator, pasa a Fase 2. Activa Producto Agent."

**Qué pasa:**
- Producto Agent lee el panorama y el brief.
- Te pregunta por value proposition, domain entities (qué cosas existen en el dominio), módulos del sistema, casos de uso principales.
- Genera:
  - `10_producto/product_detail.md`
  - `10_producto/product_elements.yaml`
  - `10_producto/versioning_policy.md`
  - Opcionalmente `wireframes/` y `catalog_view.html`.

**Punto crítico:** este agente **NO escribe detalle técnico**. Si te encuentras hablando de schemas SQL o contratos JSON, el agente debe parar y derivarte a TechSpec en la siguiente fase.

**Qué hacer después:**
1. Lee `product_detail.md` y `product_elements.yaml`. Verifica que reflejan el qué del producto sin contaminación técnica.
2. Si todo OK, guardas en `docs/10_producto/`.
3. Le dices al Orchestrator: "Producto aprobado, pasa a Fase 3".

---

## Fase 3 · TechSpec

**Lanza:** "Orchestrator, pasa a Fase 3. Activa TechSpec Agent."

**Esta es la fase más larga.** Es donde más decisiones técnicas se toman.

**Qué pasa:**
- TechSpec Agent te lleva por 14 secciones del SDD (Introducción, Visión arquitectónica, Modelo de datos, API, Pipelines, UI, Auth, ADRs, NFRs, Testing, Deployment, Migración, Riesgos, Apéndices).
- Para cada decisión arquitectónica, **te propone opciones** y registra como ADR la que elijas.
- Te avisa cuando una decisión necesitará espejo en gobernanza (el Orchestrator lo notará).
- Genera:
  - `20_especificacion_tecnica/sdd.md` (canon)
  - `api_surface.md`, `catalog_schema.md` si aplica
  - `plans/current.md` (plan de implementación con hitos numerados)
  - `deployment/{entorno}.md` por entorno

**Punto crítico:** las decisiones del SDD se registran como ADRs. **Cada ADR es candidato a tener su espejo en gobernanza** (lo verás en Fase 4).

**Qué hacer después:**
1. Lee el SDD a fondo. Es el contrato técnico exigible.
2. Si te encuentras dudando de un ADR, mejor abrir un nodo `dec-` en estado `propuesta` en Fase 4 que cerrarlo en `aceptada` con duda.
3. Guardas en `docs/20_especificacion_tecnica/`.
4. Le dices al Orchestrator: "TechSpec aprobado, pasa a Fase 4".

---

## Fase 4 · Governance

**Lanza:** "Orchestrator, pasa a Fase 4. Activa Governance Agent."

**Qué pasa:**
- Governance Agent toma los ADRs del SDD y los espeja como nodos `dec-` en `30_gobernanza/catalogo/decisiones/`.
- Genera el esqueleto:
  - `SCHEMA.md` adaptado a tu proyecto (zonas de dato según tu contexto, capas técnicas según tu stack, sistemas).
  - `LOOP.md` (patrón de 5 pasos, sin cambios).
  - `CLAUDE.md` adaptado a tu proyecto.
  - `STACK_NORMATIVO.md` aterrizando AI Act / RGPD / OWASP / ISO 42001 / NIST según aplique a tu caso.
  - `referencia/zonas.yaml`, `capas.yaml`, `horizontes.yaml`, `proyectos.yaml`, `sistemas.yaml`.
- **Genera políticas iniciales** derivadas de los ADRs (typically 5-12 políticas).
- **Genera normas** aplicables.

**Punto crítico:** la gobernanza **no nace completa**. El esqueleto inicial cubre lo arquitectónico-decisional; los nodos de catálogo (datasets, modelos, agentes, etc.) crecen progresivamente a medida que avanzas con la implementación. El Governance Agent puede sembrar algunos iniciales si los identificas en el brief.

**Qué hacer después:**
1. Revisa `SCHEMA.md` y `STACK_NORMATIVO.md` con criterio. Son los más críticos.
2. Verifica que cada ADR del SDD tiene su espejo `dec-`.
3. Guardas en `docs/30_gobernanza/`.
4. Le dices al Orchestrator: "Governance aprobado, pasa a Fase 5".

---

## Fase 5 · Reference

**Lanza:** "Orchestrator, pasa a Fase 5. Activa Reference Curator."

**Qué pasa:**
- Reference Curator te pregunta por materiales que ya tienes:
  - ¿Excel de consultor metodológico? → `40_referencia/consultores/{nombre}/`
  - ¿Datos para demo? → `40_referencia/demo/`
  - ¿User research? → `40_referencia/discovery/`
  - ¿Specs antiguas, RFP, brief original? → `40_referencia/historico/`
- Genera los `README.md` por subcarpeta explicando el contexto.
- Crea `50_bitacora_y_revision/REVISION_GUIDE.md` adaptada (itinerarios de revisión de tu proyecto).
- Si lo pides, genera `revision_review.html` con la lista de docs.

**Qué hacer después:**
1. Mueves físicamente los materiales a las carpetas correctas.
2. Guardas los READMEs generados.
3. Le dices al Orchestrator: "Reference aprobado. Proyecto listo para implementación".

---

## Iteraciones (Fase 7+)

Una vez generada toda la documentación inicial, **el equipo no termina su trabajo**. Cada vez que:

- Tomas una decisión arquitectónica nueva → Governance Agent crea nodo `dec-`.
- Generas un activo de dato/modelo/agente en la implementación → Governance Agent registra como nodo del catálogo (loop de 5 pasos).
- Cierras un hito → BITACORA se actualiza (puede usar Panorama Agent o tú directamente).
- Excel actualizado del consultor → Reference Curator clasifica.

El Orchestrator orquesta esto. Le dices: "Tenemos un nuevo dataset en H10, regístralo" y el Orchestrator activa Governance.

---

## Comandos típicos al Orchestrator

| Lo que dices | Lo que hace |
|---|---|
| `"Iniciar nuevo proyecto"` | Activa Discovery Agent |
| `"Brief aprobado, pasa a Fase N"` | Activa el agente de la fase N |
| `"Panorama necesita revisión"` | Vuelve a Panorama con tus cambios |
| `"Encontré un gap entre Producto y TechSpec"` | Detecta la contradicción y propone resolución |
| `"Tenemos un ADR nuevo en sdd.md §8 ADR-014"` | Activa Governance para espejarlo |
| `"Cerramos PRJ-Hx, actualiza BITACORA"` | Reactiva Panorama Agent (modo update) |
| `"Estado del proyecto"` | Te da el state JSON consolidado |
| `"Lista de open questions"` | Te muestra lo pendiente |

---

## Reglas de oro

1. **Una fase a la vez.** No saltes adelante hasta cerrar la actual.
2. **Lee los outputs.** No los aceptes sin leerlos.
3. **Si dudas, escala al Orchestrator.** Para eso está.
4. **Guarda inmediatamente.** Los outputs aprobados se commitean a tu repo, no se quedan en el chat.
5. **El equipo no es un oráculo.** Si te propone algo y tu intuición dice "no", confía en tu intuición y discute.

---

## Próximas lecturas

- [`03_QUICKSTART.md`](03_QUICKSTART.md) — si todavía no tienes Cowork configurado.
- [`worked_example/`](worked_example/) — caso OCA-BIM completo, fase a fase.
- [`protocols/escalation_protocol.md`](protocols/escalation_protocol.md) — cómo se ve un escalado típico.
