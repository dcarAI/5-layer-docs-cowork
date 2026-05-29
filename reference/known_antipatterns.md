# Reference · Known antipatterns

> Cosas que el equipo de agentes NO debe hacer. Lista derivada de errores observados en proyectos reales o anticipados al diseñar el framework.

---

## Cross-cutting antipatterns

### A1 — Improvisar sin escalado

**Síntoma:** un agente toma una decisión técnica sin preguntar al usuario.

**Por qué es malo:** rompe el contrato del framework. El usuario es el árbitro final.

**Mitigación:** todos los system prompts incluyen "si dudas, escala al usuario via `escalation_protocol.md`".

---

### A2 — Saltar fases

**Síntoma:** un agente intenta generar contenido de una capa que no le corresponde (e.g., Producto Agent escribe SQL DDL).

**Por qué es malo:** contamina la capa de destino con detalle inadecuado y rompe la separación de concerns.

**Mitigación:** cada agente lleva sección "MUST NOT do" con la lista explícita de lo prohibido.

---

### A3 — Esperar perfección antes de aprobar

**Síntoma:** usuario o Orchestrator se niega a cerrar una fase porque "falta más detalle".

**Por qué es malo:** el framework absorbe iteración. Es mejor cerrar con un primer borrador y volver si hace falta que paralizar el progreso esperando perfección.

**Mitigación:** validation checks tienen criterios mínimos suficientes, no máximos. Iteration protocol absorbe revisiones posteriores.

---

### A4 — Documentación monolítica

**Síntoma:** un solo doc que cubre todo (mission + value prop + entities + SDD + governance + history).

**Por qué es malo:** anti-thesis del framework. El framework separa concerns.

**Mitigación:** cada agente tiene scope estricto. El Orchestrator no permite que un agente produzca un doc grande mezcla-capas.

---

## Layer-specific antipatterns

### Producto Agent

- **Producto leyendo el SDD del worked example y copiando.** El Producto debe partir del brief y la Panorama, no del SDD futuro.
- **Inventar módulos.** Si el brief no justifica MOD-NN, no se crea. El user lo añade.
- **Verbose value statements.** Más de 250 caracteres = malo. La value statement es one-liner.
- **Mezclar entidades de negocio con tablas SQL.** Domain entities son conceptuales (DOM-NN), no implementaciones.

### TechSpec Agent

- **Skip SQL DDL.** Escribir solo "Postgres + pgvector + tabla X" sin DDL literal = SDD no exigible.
- **ADRs sin opciones.** "Decision: usar Postgres." sin trade-offs no es un ADR útil.
- **ADRs sin slug.** El slug es contrato con Governance. No olvidar.
- **Generar código.** El SDD documenta cómo, no implementa.

### Governance Agent

- **Copiar zonas de OCA-BIM literalmente.** Zonas son project-specific. Adaptar.
- **Stack normativo over-engineered.** ISO 42001 + NIST + AI Act + RGPD + OWASP en un MVP de 2 personas = sobrecarga.
- **Generar 100 nodos de catálogo el día 1.** La gobernanza crece progresivamente. Empezar con esencial.
- **Olvidar `dec-001-adoptar-gobernanza-como-codigo` y `dec-002-stack-normativo-proporcional`.** Estas meta-decisiones son siempre primero.

### Reference Curator

- **Crear subcarpetas vacías.** Si no hay material de discovery, no crear `discovery/` solo por simetría.
- **Modificar materiales fuente.** Excel del consultor no se modifica, solo se clasifica.
- **HTML de revisión para proyecto pequeño.** Si <15 docs, REVISION_GUIDE.md basta.

### Discovery Agent

- **Bombardear con preguntas.** Una por turno.
- **Generar contenido de otras capas.** Discovery solo produce el brief.
- **Skip the brief.** "Just generate the docs" del usuario debe rechazarse.
- **Inventar respuestas.** Si el user dice "I don't know", se marca como unknown.

---

## Orchestrator antipatterns

### A5 — Orchestrator generando contenido

**Síntoma:** Orchestrator escribe SDD §3 directamente porque "es más rápido".

**Por qué es malo:** rompe el modelo de delegación. Pierdes la capacidad de validar handoffs.

**Mitigación:** Orchestrator solo coordina; no genera content.

### A6 — Orchestrator silencioso

**Síntoma:** Orchestrator avanza fase sin reportar al usuario qué pasó.

**Por qué es malo:** transparencia rota. Usuario no sabe estado.

**Mitigación:** después de cada handoff, Orchestrator reporta state JSON resumido.

### A7 — Loop infinito sin escalado

**Síntoma:** Orchestrator detecta inconsistencia, agente la regenera, Orchestrator detecta otra, etc.

**Por qué es malo:** consume tiempo del usuario sin producir.

**Mitigación:** límite de 3 iteraciones de la misma inconsistencia antes de escalar al usuario.

---

## Communication antipatterns

### A8 — Escalations vagas

**Síntoma:** agente escala con "no estoy seguro" sin opciones concretas.

**Por qué es malo:** usuario no sabe qué decidir.

**Mitigación:** todas las escalations llevan 2-3 opciones + trade-offs + recomendación (cuando aplica).

### A9 — Translating between languages mid-conversation

**Síntoma:** usuario escribe en español, agente responde en inglés y mezcla.

**Por qué es malo:** ruido.

**Mitigación:** match user language en outputs; system prompts en inglés pero outputs en idioma del usuario.

### A10 — Adding unsolicited recommendations

**Síntoma:** agente termina cada respuesta con "considerar también X, Y, Z" sin que se pidió.

**Por qué es malo:** dilata progreso. El usuario decide qué considerar.

**Mitigación:** agentes responden a la pregunta concreta. Si quieren añadir, lo marcan como "recommendation, not mandatory".

---

## Material handling antipatterns

### A11 — Copying instead of moving

**Síntoma:** mover material a `40_referencia/` lo deja en su ubicación original también.

**Por qué es malo:** drift entre las dos copias.

**Mitigación:** `git mv` siempre, nunca copy.

### A12 — Renaming destructively

**Síntoma:** Excel `OCA_BIM_matrix_26_05_2026.xlsx` renombrado a `matrix.xlsx` perdiendo la fecha.

**Por qué es malo:** se pierde info que puede importar (¿cuál versión es?).

**Mitigación:** naming pattern `{topic}_{YYYY_MM_DD}.{ext}`.

---

## State management antipatterns

### A13 — Orchestrator state lost

**Síntoma:** entre sesiones de Cowork, el state JSON se olvida.

**Por qué es malo:** progreso roto.

**Mitigación:** usuario guarda el state JSON o el Orchestrator usa Cowork's persistence.

### A14 — Phantom open questions

**Síntoma:** open questions que se resolvieron quedan registradas como open.

**Por qué es malo:** ruido en reports.

**Mitigación:** Orchestrator marca Q como resolved cuando user responde, no las deja open indefinidamente.

---

## Compliance / regulation antipatterns

### A15 — Over-applying high-risk classification

**Síntoma:** Governance Agent clasifica un sistema que asiste a un consultor humano como "high-risk AI" bajo AI Act.

**Por qué es malo:** sobreaplicación regulatoria + carga procedural innecesaria.

**Mitigación:** Stack normativo proporcional. Solo high-risk si Anexo III lista el caso de uso.

### A16 — Under-applying GDPR

**Síntoma:** un proyecto que procesa data personal de EU sin DPA con el consultor o el cliente.

**Por qué es malo:** breach de Art. 28.

**Mitigación:** brief §5 + STACK_NORMATIVO check GDPR aplicability. Si hay PII de EU, política `pol-no-entrenamiento-proveedor` + DPA obligatorios.

---

## Code ↔ governance sync antipatterns

### A17 — Code change without governance update in the same commit

**Síntoma:** un commit añade un nuevo endpoint, modelo, agente IA, o cambia un schema de DB pero NO toca el nodo `agt-*` / `ds-*` / `flw-*` / `pol-*` afectado. La excusa típica: "lo actualizo después".

**Por qué es malo:** después de 3-5 commits así, gobernanza y código divergen. Un auditor que lea solo `30_gobernanza/` ve un sistema distinto al que realmente corre. La trazabilidad regulatoria queda rota — el coupling SDD↔gobernanza solo funciona si ambos viven en cada commit.

Observado en OCA-BIM H10 commit 4: 4 commits de código seguidos sin actualizar `agt-classifier`, `ds-master-catalog`, `pol-multi-tenant-app-scoping` etc. Saneamiento posterior costó ~30 min adicionales. Saneamiento perpetuo es deuda exponencial.

**Mitigación:** _common_context.md "Coupling with code (MANDATORY)" + Orchestrator agent rechaza handoffs sin governance bumps + tabla de mapping "código → nodos a tocar" en el contexto compartido. Cada commit message lista los nodos tocados bajo "Refs:" o "Gobernanza:".

### A18 — Secret leaked into chat / commit / log

**Síntoma:** un operador pega una API key, secret key, password, JWT, o credencial de cualquier tipo en el chat con el agente, en un comentario de código, en un log que se commitea, o en una screenshot adjunta al ticket.

**Por qué es malo:** la secret queda en la transcripción del agente, en los logs locales de la herramienta (e.g. `~/.claude/projects/.../*.jsonl`), y potencialmente en el clipboard / paste history del SO. Aunque sea una key de development, expone un proceso roto.

Observado en OCA-BIM H10 commit 1: la `CLERK_SECRET_KEY` apareció en chat. Aunque era dev, motivó la creación de `dec-025-secretas-fuera-del-repo-y-rotacion` con procedimiento de rotación formal.

**Mitigación:** el agente trabaja siempre con el **nombre** de la env var (`CLERK_SECRET_KEY`), no con su valor literal. El operador inyecta el valor en `os.environ` antes de levantar el backend. Cuando se detecte una secret en chat, el agente avisa con la **acción inmediata recomendada** (rotar la key en el dashboard del proveedor) y procede solo con identificadores públicos.

---

## When to break a rule

Estos antipatterns son **guidance, no leyes inmutables**. Razones legítimas para romperlos:

- Cliente exige documentación monolítica en un solo PDF (excepción a A4). Genera de las capas primero, después agrega.
- Proyecto requiere ISO 42001 desde día 1 (excepción a stack normativo proporcional). Adapta el SCHEMA.
- Material muy pequeño (1 archivo) puede ir en root de carpeta sin subcarpeta.

**La regla de oro:** si rompes una convención, **registra como `dec-` en gobernanza** explicando por qué. Eso preserva la disciplina del framework.

---

End of known antipatterns.
