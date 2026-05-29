# Reference · Conventions extract

> Convenciones de naming, versionado, idioma y organización interna. Para detalle completo ver el `_framework/conventions.md` del repo OCA-BIM.

---

## Naming de carpetas de capa

`{NN}_{name_in_user_language}` con NN rellenado a 2 dígitos.

Estándar:
- `00_panorama/`
- `10_producto/`
- `20_especificacion_tecnica/`
- `30_gobernanza/`
- `40_referencia/`
- `50_bitacora_y_revision/`

Opcionales (huecos de 10):
- `05_origen/` — pitch original, RFP, brief inicial.
- `15_branding/` — identidad visual.
- `25_qa/` — plan de QA si es muy grande.
- `35_compliance/` — si compliance supera a gobernanza.
- `45_marketing/` — sales enablement.
- `55_postmortem/` — lecciones aprendidas.
- `60_observabilidad/` — runbooks, dashboards, alertas (post-operación).

**Reservado:** `_framework/` para el meta-framework (siempre con prefijo `_`).

---

## Naming de archivos canon

| Archivo | Significado |
|---|---|
| `README.md` | Entry point de la carpeta |
| `INDEX.md` | Mapa navegable (solo en root de `docs/`) |
| `BITACORA.md` | Estado vivo (solo en `00_panorama/`) |
| `current.md` | Vigente de evolución (en `plans/` u otras) |
| `archive/{version}.md` | Supersedido (mantener filename = versión) |

---

## Naming de archivos descriptivos

`snake_case` o `kebab-case` consistente dentro de la capa.

Ejemplos OCA-BIM (castellano body, inglés identificadores):
- `product_detail.md` (no `OCA_BIM_PRODUCT_DETAIL_v0.md`)
- `sdd.md` (no `SDD_v0_5.md`)
- `versioning_policy.md` (no `VERSIONING.md`)
- `staging.md` (no `plan_despliegue_staging.md`)

**Anti-patrón:** versión en filename. Versionar en cabecera o front-matter. Excepción: archivos de archivo (`archive/v0_3.md`) donde la versión es identidad.

---

## Versionado de docs

SemVer en cabecera de cada documento canon:

| Tipo | Patch | Minor | Major |
|---|---|---|---|
| Producto | Typo, traducción | Nueva sección | Restructuración |
| SDD | Aclaración técnica | Nuevo ADR | Cambio arquitectónico |
| Plan migración | Reorden interno | Hito añadido | Plan superpuesto |
| Nodos gobernanza | Reword | Atributo añadido | Cambio estructura |

**Cualquier bump MAJOR requiere un nodo `dec-` en gobernanza** que lo justifique.

---

## Versionado del propio framework

Si adaptas el framework para tu proyecto y los cambios son estructurales (añadir capa, cambiar regla), bumpa la versión del framework en `_framework/README.md` y documenta en su changelog.

---

## Idioma

**Decisión por proyecto, consistente dentro de cada capa.**

OCA-BIM:
- **Español**: cuerpo docs, narrativa, bitácora, glosario.
- **Inglés**: código, schemas técnicos, identificadores, ENUMs, mensajes error, commits.
- **PT-PT**: briefing a Manuel específico.

Recomendación general:
- Cuerpo en idioma del equipo.
- Identifiers técnicos en inglés.

---

## Estructura interna por capa

### 00_panorama/

```
00_panorama/
├── README.md
├── BITACORA.md
└── stakeholders.md   # opcional
```

### 10_producto/

```
10_producto/
├── README.md
├── product_detail.md          # canon
├── product_elements.yaml      # machine-readable
├── catalog_view.html          # render opcional
├── versioning_policy.md
├── wireframes/                # opcional
└── personas/                  # opcional
```

### 20_especificacion_tecnica/

```
20_especificacion_tecnica/
├── README.md
├── sdd.md                     # canon
├── api_surface.md             # si hay API
├── catalog_schema.md          # si hay catálogo
├── plans/
│   ├── README.md
│   ├── current.md
│   └── archive/
└── deployment/
    ├── README.md
    └── {entorno}.md
```

### 30_gobernanza/

Ver estructura detallada en `_framework/templates/30_gobernanza/README.md`.

### 40_referencia/

```
40_referencia/
├── README.md
├── discovery/             # subcarpetas según subdominio
├── consultores/{nombre}/
├── demo/
└── historico/
```

### 50_bitacora_y_revision/

```
50_bitacora_y_revision/
├── README.md
├── REVISION_GUIDE.md
├── revision_review.html         # opcional
├── revisiones/{fecha}.json      # opcional
└── changelogs/                  # opcional
```

---

## Reglas de mantenimiento

1. **Cada carpeta tiene README.md.**
2. **Material supersedido se archiva**, no se borra. `archive/` o `historico/`.
3. **Cambios estructurales mayores** (nueva capa, renombrado) → nodo `dec-` en gobernanza.
4. **INDEX.md se actualiza manualmente** al añadir docs nuevos al ámbito principal.
5. **BITACORA.md se actualiza al cierre de hitos**, no en cada commit.
6. **Templates en `_framework/templates/` sincronizados** con la práctica real.

---

End of conventions extract.
