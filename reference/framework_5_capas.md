# Reference · Framework de 5 Capas (resumen)

> Resumen autocontenido del framework, listo para que cualquier agente lo lea como referencia rápida. Más detallado que `_common_context.md` pero más conciso que las plantillas completas.

---

## Las 5 capas

| Capa | Propósito | Audiencia primaria |
|---|---|---|
| `00_panorama/` | Contexto estable + bitácora viva | Cualquier colaborador nuevo |
| `10_producto/` | Definición del producto: qué hace, para quién | PM, comprador, CEO en demo |
| `20_especificacion_tecnica/` | Cómo se construye: SDD, schemas, planes técnicos | Ingeniero, Claude Code en sesiones de implementación |
| `30_gobernanza/` | Gobernanza como código: schema + decisiones + políticas + normas | Auditor regulatorio, DPO, Claude Code |
| `40_referencia/` | Material externo: consultores, demo, histórico | Variable según subcarpeta |
| `50_bitacora_y_revision/` | Revisión y trazabilidad de sesiones | Tú revisando antes de codear o de gates |

---

## Dependencias direccionales

```
00_panorama (contexto estable)
    │
    ▼
10_producto (qué hace)
    │
    ▼
20_especificacion_tecnica (cómo se construye)
    │
    ▼
30_gobernanza ←─── referencia desde todas las capas
    ▲
    │
    │
40_referencia, 50_bitacora_y_revision (capas de soporte)
```

**Regla:** una capa puede leer de las anteriores, no de las posteriores. La gobernanza es transversal pero solo **crece progresivamente** (no nace completa el día 1).

---

## Por qué este patrón

Sin estructura, los proyectos terminan con `docs/` plano donde:
- PM no encuentra el value proposition entre los DDL SQL.
- Ingeniero pierde tiempo leyendo catálogos de stakeholders.
- Auditor regulatorio no encuentra dónde están los controles AI Act.
- Colaborador nuevo no sabe dónde empezar.

El framework de 5 capas resuelve esto por **separación de concerns** y **dependencias visibles**.

---

## Convenciones críticas

### Naming de carpetas

`{NN}_{nombre}` con NN rellenado a 2 dígitos: `00_panorama/`, `10_producto/`, `20_especificacion_tecnica/`, `30_gobernanza/`, `40_referencia/`, `50_bitacora_y_revision/`. Huecos de 10 para insertar futuras (`05_`, `15_`, etc.).

### Naming de archivos

- `README.md` punto de entrada de carpeta.
- `INDEX.md` solo a nivel root de `docs/`.
- `current.md` versión vigente de docs evolutivos.
- `archive/{version}.md` supersedidos.

### Prefijos de nodos de gobernanza

```
dec-  decision         pol-  política           norm- norma
ds-   dataset          mdl-  modelo IA          agt-  agente
flw-  flujo            task- tarea              uc-   caso uso
dom-  dominio          term- término            prm-  prompt    eval- evaluación
```

### Idioma

Decisión por proyecto:
- **OCA-BIM y similares**: español body, inglés código/identificadores/system prompts.
- **Mensalus**: español body, claves YAML en castellano (decisión específica).
- **Inglés-first**: todo en inglés.

---

## El loop de 5 pasos (gobernanza)

Aplicar a cada nuevo activo, decisión, modelo, agente o prompt:

1. **Identificar** — asignar id, declarar tipo.
2. **Clasificar** — `mapeo` + facetas.
3. **Evaluar** — semáforo, riesgo IA, EIPD/FRIA.
4. **Registrar** — commit del nodo + dec- si aplica.
5. **Controlar** — política derivada o referenciada.

**Modos:**
- **Bloqueante**: decisiones nuevas, modelos, agentes, prompts, datasets Z2/Z3, sistemas externos.
- **Flexible** (saltar 3 y 5 si justificado): activos Z1/Z4 rutinarios siguiendo patrón establecido.

---

## Coupling SDD ↔ gobernanza (regla de oro)

Cada ADR del `sdd.md §8` tiene su espejo en `30_gobernanza/catalogo/decisiones/dec-NNN-{slug}.md` con `espeja_adr: ADR-NNN`.

- El SDD lleva la narrativa.
- El nodo lleva el estado, trayectoria, aristas con otros nodos.
- **Cero duplicación de contenido**; sí duplicación de trazabilidad estructural.

---

## Stack normativo proporcional

| Stack | Cuándo aplicar |
|---|---|
| EU AI Act Art. 50 transparencia | Deployer/provider de IA en EU con usuario final humano |
| GDPR | EU + datos personales procesados |
| OWASP LLM Top 10 | Cualquier uso de LLM en aplicación |
| OWASP Agentic Top 10 2026 | Agentes con autonomía o herramientas |
| ISO/IEC 42001 | Solo si cliente o regulador lo exige |
| NIST AI RMF + GenAI Profile | Solo si stakeholder US-tier lo pide |
| HIPAA / sector-specific | Solo si aplica al sector |

**Regla de pulgar:** lo mínimo + OWASP es suficiente para MVP. ISO y NIST se difieren hasta cliente real que los exija.

---

## Lo que NO está en ningún agente

- Generar código de aplicación.
- Diseñar UI/UX visual.
- Decisiones de negocio (pricing, partnerships).
- Negociación con consultores o terceros.
- Implementación o tests.

Si una solicitud cae fuera, el agente escala al usuario.

---

End of reference.
