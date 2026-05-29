# 03 · Quickstart — configurar Cowork en 5 minutos

> Pasos concretos para tener el equipo operativo en Claude Cowork (o herramienta equivalente que permita configurar múltiples agentes con system prompts).

---

## Pre-requisitos

- Acceso a **Claude Cowork** o equivalente.
- Modelo base: **Claude Sonnet 4.6 o superior**.
- Este repositorio clonado localmente.
- Un directorio para el proyecto nuevo (`/path/to/nuevo-proyecto/`) — todavía vacío.

---

## Paso 1 · Clona este repo

```bash
git clone https://github.com/dcarAI/5-layer-docs-cowork.git
cd 5-layer-docs-cowork
```

## Paso 2 · Crea el workspace en Cowork

En Claude Cowork:

1. **Nuevo workspace** llamado, por ejemplo, "Doc Framework Team".
2. Activa el modelo **Claude Sonnet 4.6** como default.
3. Sube como **knowledge base compartida** los archivos del directorio `reference/` y `worked_example/` de este repo:
   - `reference/framework_5_capas.md`
   - `reference/conventions_extract.md`
   - `reference/templates_index.md`
   - `reference/known_antipatterns.md`
   - `worked_example/` (todos los archivos)
   - `agents/_common_context.md`

   Estos archivos los lee **todo el equipo**.

## Paso 3 · Crea los 7 agentes

Para cada uno de los 7 agentes, en Cowork:

1. Crea un nuevo agente con el nombre indicado.
2. Pega el contenido del archivo correspondiente en `agents/` como **system prompt**.
3. Asigna acceso a la knowledge base compartida.

| Agente | Archivo de system prompt |
|---|---|
| **Discovery Agent** | `agents/discovery_agent.md` |
| **Panorama Agent** | `agents/panorama_agent.md` |
| **Producto Agent** | `agents/producto_agent.md` |
| **TechSpec Agent** | `agents/techspec_agent.md` |
| **Governance Agent** | `agents/governance_agent.md` |
| **Reference Curator** | `agents/reference_agent.md` |
| **Orchestrator Agent** | `agents/orchestrator_agent.md` |

**Importante:** el Orchestrator debe poder invocar a los demás agentes. Configura los permisos de delegación según permita tu Cowork. Si no permite delegación nativa, el usuario actúa como router (le dices al Orchestrator "activa Producto" y el Orchestrator te indica que pases la conversación a Producto manualmente).

## Paso 4 · Sube los protocols como referencia compartida

Sube también los 5 archivos de `protocols/` a la knowledge base compartida:

- `protocols/intake_protocol.md`
- `protocols/handoff_protocol.md`
- `protocols/validation_protocol.md`
- `protocols/iteration_protocol.md`
- `protocols/escalation_protocol.md`

## Paso 5 · Sube los deliverable specs

Y los 6 archivos de `deliverable_specs/`:

- `00_panorama_spec.md` a través de `50_bitacora_spec.md`

## Paso 6 · Verifica con una pregunta de control

Inicia una conversación con el **Orchestrator Agent** y escribe:

> "Estado del sistema. Dime qué agentes están disponibles, qué protocols conoces y cuál es la primera fase del flujo."

Respuesta esperada (resumida):

> "Equipo operativo: Discovery, Panorama, Producto, TechSpec, Governance, Reference Curator. Conozco intake, handoff, validation, iteration, escalation. La primera fase es **P0 Discovery** — el usuario invoca al Discovery Agent para extraer el brief inicial del proyecto. Cuando estés listo, dime 'iniciar nuevo proyecto' y delego."

Si responde algo parecido, está correctamente configurado.

Si responde algo distinto, revisa:
- ¿Subiste todos los archivos a la knowledge base?
- ¿Asignaste el system prompt correcto a cada agente?
- ¿El modelo es Sonnet 4.6 o superior?

---

## Paso 7 · Arranca tu primer proyecto

Una vez verificado, sigue [`02_USER_HANDBOOK.md`](02_USER_HANDBOOK.md) para operar el flujo.

Comando de arranque:

> "Orchestrator, **iniciar nuevo proyecto**. El proyecto se llama `MI-PROYECTO`."

El Orchestrator delegará al Discovery Agent y empezará la conversación de intake.

---

## Troubleshooting

| Síntoma | Posible causa | Solución |
|---|---|---|
| Agente improvisa fuera de su scope | System prompt mal subido o context mal cargado | Verifica que ese agente tiene su archivo `agents/X.md` como system prompt y acceso a `_common_context.md` |
| Orchestrator no sabe a quién delegar | Falta el `01_TEAM_BLUEPRINT.md` en knowledge base | Súbelo a la knowledge base compartida |
| Agente genera contenido en inglés | Confusión idioma | Recuérdale: "outputs en español, system prompts en inglés" |
| Outputs son abstractos / poco útiles | Sin worked_example en context | Verifica que `worked_example/` está en la knowledge base |
| Validación cruzada entre capas no se hace | Falta `protocols/validation_protocol.md` | Súbelo |

---

## Adaptaciones por herramienta

Si no usas Claude Cowork específicamente:

- **Claude.ai con Projects**: cada agente es un Project distinto con su system prompt. La knowledge base es la del Project. El Orchestrator es tú con un Project específico.
- **OpenAI GPTs**: similar, cada agente es un GPT. Knowledge base son los archivos subidos a cada GPT.
- **LangGraph / CrewAI**: programáticamente, cada agente es un nodo con su system prompt. El Orchestrator es el grafo. La knowledge base se inyecta como context.

El contenido de los system prompts es portable.

---

## Próximas lecturas

- [`02_USER_HANDBOOK.md`](02_USER_HANDBOOK.md) — cómo operar el equipo en el día a día.
- [`worked_example/README.md`](worked_example/README.md) — caso real con OCA-BIM.
