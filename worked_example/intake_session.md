# Worked example — Intake session (OCA-BIM)

> Synthetic reconstruction of how the Discovery Agent's conversation went when extracting the OCA-BIM brief. Not verbatim from history; reconstructed for pedagogical purposes.

---

## Phase A — Opening

**Discovery Agent:** Hola. Soy el Discovery Agent. Vamos a empezar un proyecto nuevo. Te haré 15-20 preguntas para extraer un brief estructurado. Tardaremos ~30 min. Al final el brief sirve a todos los agentes downstream. ¿Cómo se llama el proyecto?

**User:** OCA-BIM™.

**DA:** ¿OCA-BIM es un acrónimo?

**User:** Operating Culture Acceleration & Behavior Impact Measurement.

**DA:** Recibido. Te haré las preguntas agrupadas por tema. Empezamos con identidad y misión.

---

## Phase B — Identity & mission

**DA:** En una frase, ¿qué hace OCA-BIM?

**User:** Detecta comportamientos de liderazgo en reuniones de Comité de Dirección, los cuantifica y traduce a impacto P&L en USD.

**DA:** ¿Qué problema resuelve que no se resolvía antes?

**User:** La consultoría tradicional de cambio cultural no habla el idioma del CFO. Resultados en perceptions, no en euros. Manuel Macedo lleva 20 años haciendo este trabajo a mano; queremos materializarlo en plataforma.

**DA:** ¿Quién es Manuel Macedo respecto a este proyecto?

**User:** Es el dueño metodológico. AgileFOCUS, Lisboa. El Behavior Impact Library (BIL) es suyo, propietario. Yo soy TechBizDesign — arquitecto técnico.

**DA:** ¿Organización que posee el proyecto?

**User:** AgileFOCUS + TechBizDesign en colaboración. AgileFOCUS lleva metodología, yo llevo construcción.

---

## Phase C — Audiences

**DA:** ¿Usuario primario más importante?

**User:** Tres audiencias distintas:
1. CEO en demo (10-30 min) — debe entender el valor sin que se lo expliquen.
2. CHRO o COO como comprador interno — necesita navegar a la evidencia textual.
3. Manuel y consultores de AgileFOCUS — herramienta de trabajo, no vitrina.

**DA:** ¿Decisiones de compra las toma el CHRO o el CEO?

**User:** El CHRO/COO arranca, el CEO firma.

**DA:** ¿Otros stakeholders mencionables?

**User:** Sí — futuros consultores partner que opera el sistema bajo licencia. Y eventualmente CFO del cliente que valida cifras P&L.

---

## Phase D — Phase of the project

**DA:** ¿Estado actual del proyecto?

**User:** MVP en pivote. Tenemos H1-H6 implementados con un modelo del BIL v1 (10 scenarios, 60 behaviors). Manuel acaba de entregar matriz v2 (5 clusters, 27 behaviors) y queremos re-arrancar la implementación bajo el nuevo modelo.

**DA:** Plazo crítico próximo?

**User:** En 4-5 semanas queremos tener staging operativo con demo de Demo Industrial Group y Manuel firmando off.

**DA:** ¿Hay alguna fecha externa (cliente que paga, gate regulatorio)?

**User:** No, todavía no hay cliente real. Es pre-comercial.

---

## Phase E — Domain & regulation

**DA:** ¿Industria?

**User:** Consultoría de cambio cultural ejecutivo. Aplicable a empresas medianas y grandes con LTs.

**DA:** ¿Despliegue geográfico?

**User:** Inicialmente EU. AgileFOCUS está en Lisboa, primer cliente esperable en España o Portugal.

**DA:** ¿AI Act aplica?

**User:** Sí en cuanto deployer de IA. ¿Pero somos alto riesgo? Las detecciones del LLM informan al consultor; el humano decide. Creo que es tier transparencia (Art. 50).

**DA:** Anotado. Con Digital Omnibus de mayo 2026 efectivamente queda como transparencia. ¿GDPR?

**User:** Sí. Procesamos transcripts de reuniones de Comité de Dirección — datos personales corporativos. No Art. 9 salvo casos excepcionales (mención de salud o creencias en un transcript).

**DA:** ¿DPA con clientes y con Anthropic?

**User:** Anthropic ya tiene cláusula no-training. DPA con cliente: por redactar cuando haya cliente real.

---

## Phase F — Tech stack

**DA:** Backend?

**User:** Python 3.12 + FastAPI + SQLAlchemy 2.

**DA:** Frontend?

**User:** Next.js 16 + React 19 + Tailwind v4 + shadcn/ui + next-intl.

**DA:** Base de datos?

**User:** PostgreSQL 16 + extensión pgvector.

**DA:** Cloud target?

**User:** Staging en GCP — Compute Engine + Docker Compose. Producción sin decidir.

**DA:** Auth?

**User:** Migración pendiente de API keys a Clerk con organizations + capabilities.

**DA:** LLM provider?

**User:** Anthropic. Haiku 4.5 para clasificación, Sonnet 4.6 para detección core. Opus 4.7 para Premium (Batch API) y Calibration mode. Voyage AI para embeddings (cableado diferido).

**DA:** ¿Otros servicios externos?

**User:** Redis 7 como cola (inactivo hasta H15). GitHub Actions para CI.

---

## Phase G — Existing materials

**DA:** ¿Excel o docs de Manuel?

**User:** Sí, tres entregas. La definitiva es `OCA_BIM_matrix_26_05_2026.xlsx`. Las dos anteriores son históricas inertes.

**DA:** ¿Specs antiguas?

**User:** Tengo 9 .docx originales del MVP H1-H6 (arquitectura, modelo datos, etc.). Y un prototipo HTML que muestra el dashboard.

**DA:** ¿Datos demo?

**User:** Plan escrito para Demo Industrial Group: 12 transcripts sintéticos, 11 ejecutivos, arco narrativo de mejora cultural en 3 meses. No generados todavía.

---

## Phase H — Roles

**DA:** ¿Roles definidos?

**User:** Yo arquitecto técnico y orquestador. Manuel dueño metodológico. Sin DPO formal — yo cubro el rol hasta que firmemos cliente real con compliance officer.

---

## Phase I — Constraints

**DA:** ¿Restricciones críticas?

**User:** Budget self-funded, sin headcount adicional. Solo yo + Manuel en horas marcadas. Hard deadline: staging vivo en 4-5 semanas. Out of scope: integración con Zoom/Teams (post-MVP), aplicación móvil, multi-region.

---

## Phase J — Prior decisions

**DA:** ¿Decisiones ya firmadas que registrar?

**User:** Varias:
- Anthropic Standard como provider LLM de arranque.
- Clerk como provider de auth.
- GCP Compute Engine para staging.
- Constitutional Retrieval como patrón de detección.
- Stack normativo proporcional: AI Act Art. 50 + OWASP, sin ISO/NIST en MVP.
- Modelo behavior-centric con KPI valence (decidido el 28 mayo con Manuel).

**DA:** ¿Algo en estado "propuesta" o "pendiente de decidir"?

**User:** El método de agregación de impacto monetario (Gaussian vs SUM). Decisión final con Manuel en gate H15/H16.

---

## Phase K — Open questions for later

**DA:** ¿Cosas que no sabes responder ahora?

**User:** Cifras P&L calibradas del catálogo — pendientes de CFO de futuro cliente. Manuel y CFO completan post-publicación del catálogo.

---

## Phase L — Brief synthesis

**DA:** [presenta el brief completo en formato `intake/brief.md`]

[1 minute pause]

**DA:** ¿Apruebas el brief?

**User:** Falta una cosa en §3: añadir que las traducciones de los signals del catálogo a ES/PT son responsabilidad de Manuel, no del equipo técnico.

**DA:** Recogido. [actualiza §3] ¿Algo más?

**User:** No, está completo.

**DA:** Brief approved. Saved at `intake/brief.md`. Signaling Orchestrator to advance to Phase 1.

---

## Time elapsed

~35 minutes. 22 main questions + ~10 follow-ups.

---

## Observations

- User came with **strong prior context** (already had architectural decisions). Discovery did not need to drill on basics.
- One **important addendum** in synthesis phase (the translation responsibility). This kind of late-stage addition is normal.
- Brief was **rich in §11 (prior decisions)** — 6 decisions already made. These become 6 starting `dec-` nodes in P4.
- **No mid-conversation confusion** about audiences or mission — fast convergence.

For projects starting "from scratch" with less prior context, expect Discovery to take 60+ minutes and more drilling questions.

---

End of intake session.
