# Deliverable Spec · 10_producto/

> Defines what the Producto Agent must produce.

---

## Mandatory outputs

### 1. `10_producto/product_detail.md`

**Purpose:** canonical product definition. Forrest PMP v0 (Product Master Plan) format adapted to project context.

**Length:** typically 300-800 lines depending on product complexity.

**Required sections:**
- 0. How to read (IDs, conventions).
- 1. Value Proposition (value statement + differential values).
- 2. Architecture (conceptual, high-level — NOT technical).
- 3. Domain entities (DOM-NN list).
- 4. Product Elements (MOD-NN modules, SM-NN submodules, FN-NN functionalities, SRV-NN services, EXT-NN externals).
- 5. Stakeholders.
- 6. High-level requirements (functional + non-functional + product/business).
- 7. Cross-references.
- 8. Global product maturity (TRL level).
- Appendix A: catalog conventions.

**Forbidden:**
- SQL DDL.
- JSON API contracts.
- Pseudocode.
- Sequence diagrams (implementation-level).
- Regulatory deep-dive (that goes in `30_gobernanza/STACK_NORMATIVO.md`).

### 2. `10_producto/product_elements.yaml`

**Purpose:** machine-readable source of truth corresponding to `product_detail.md`.

**Length:** typically 500-2500 lines depending on product elements count.

**Structure (top-level):**
```yaml
schema: { name, version, description }
document: { product_name, forrest_phase, version, status, language, dates, owners }
changelog: [...]
domain_entities: [...]
elements: [...]   # modules, submodules, functionalities, services, externals
```

**Rule of precedence:** when YAML and MD differ, the YAML wins. The Producto Agent generates them in sync.

### 3. `10_producto/versioning_policy.md`

**Purpose:** policy document describing SemVer for product docs.

**Length:** ~50-150 lines.

**Required sections:**
- Scope (which docs governed).
- SemVer rules (PATCH/MINOR/MAJOR).
- Bump triggers per doc type.
- How to record changes (changelog format).

---

## Optional outputs

### 4. `10_producto/wireframes/`

Generate **only if the project has UX/UI** and the brief or user mentions visual flows.

Files:
- `navigation.html` — standalone HTML wireframe with CSS, showing app layout and navigation between screens. Pattern: 5-layer-docs-cowork worked_example shows the OCA-BIM example.
- Other mockup files as needed.

### 5. `10_producto/catalog_view.html`

If the product has a structured catalog (e.g., methodological), generate an HTML render of `product_elements.yaml` for visual consultation.

### 6. `10_producto/personas/`

If the brief §3 audiences are complex, generate persona files.

---

## Acceptance criteria

- [ ] `product_detail.md` value statement is one sentence and <250 chars.
- [ ] At least 1 MOD-NN defined (otherwise the product has no software components and this framework doesn't apply).
- [ ] At least 1 DOM-NN defined.
- [ ] Stakeholders consistent with brief §8 + Panorama audiences.
- [ ] `product_elements.yaml` parses as valid YAML.
- [ ] `product_elements.yaml` ID prefixes consistent: MOD-/SM-/FN-/DOM-/SRV-/EXT-.
- [ ] Same DOM-NN list in MD §3 and YAML `domain_entities`.
- [ ] Same MOD-NN list in MD §4 and YAML `elements`.
- [ ] No SQL, no JSON contracts, no implementation pseudocode anywhere.
- [ ] User approved.

---

## Common mistakes to avoid

- **Verbose value statements.** Keep value to 200 chars max.
- **Inventing modules.** If brief doesn't justify a MOD, don't create it. Wait for user.
- **Technical leakage.** "Postgres" appears only as a stack mention; not as a design constraint.
- **TRL inflation.** Be honest about maturity. Most pre-implementation projects are TRL 2-3.
