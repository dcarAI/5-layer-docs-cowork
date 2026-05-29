# Reference · Templates index

> Punteros a las plantillas vacías del framework. Los agentes las usan como punto de partida cuando generan docs.

---

## Where templates live

Los templates están en el repo OCA-BIM en `/_framework/templates/`:

```
_framework/templates/
├── 00_panorama/
│   ├── README.md
│   └── BITACORA.md
├── 10_producto/
│   └── README.md
├── 20_especificacion_tecnica/
│   ├── README.md
│   ├── plans/
│   └── deployment/
├── 30_gobernanza/
│   ├── README.md
│   ├── referencia/
│   └── catalogo/decisiones/
├── 40_referencia/
│   └── README.md
└── 50_bitacora_y_revision/
    └── README.md
```

---

## Cómo usarlas

### Si estás replicando el framework en proyecto nuevo

```bash
cp -r /path/to/oca-bim/_framework/templates/* /path/to/nuevo-proyecto/docs/
```

Esto deja la estructura base. Cada README es placeholder.

### Si eres un agente generando contenido

1. Lee el template correspondiente como **punto de partida estructural**.
2. **Reemplaza los placeholders** (`{PLACEHOLDER}`) con contenido real del brief/proyecto.
3. **No copies literalmente** — adapta tono, longitud y detalle al proyecto.
4. **Mantén las secciones obligatorias** del template, omite las opcionales si no aplican.

---

## Template fingerprints

### `00_panorama/README.md` template

Sections:
1. Misión
2. Audiencias (tabla)
3. Stack y tecnologías
4. Roles y stakeholders
5. Estado del proyecto (link a BITACORA)
6. Regulación aplicable (resumen, link a STACK_NORMATIVO)
7. Materiales existentes (link a 40_referencia)

### `00_panorama/BITACORA.md` template

8 secciones estandarizadas:
0. Última actualización
1. Cómo retomar trabajo en sesión nueva
2. Estado por hito
3. Gates externos pendientes
4. Decisiones cerradas recientes
5. Próximo entregable concreto
6. Riesgos abiertos críticos (top 3)
7. Glosario rápido
8. Cómo actualizar esta bitácora

### `10_producto/README.md` template

Punteros a archivos típicos + reglas de mantenimiento.

### `20_especificacion_tecnica/README.md` template

Punteros a SDD, plans, deployment + reglas de coupling con gobernanza.

### `30_gobernanza/README.md` template

**Especial:** este template tiene instrucciones de adaptación porque la gobernanza requiere más decisiones project-specific (zonas, normas, etc.).

### `40_referencia/README.md` template

Subfolder taxonomy + reglas de mantenimiento.

### `50_bitacora_y_revision/README.md` template

Aclaración de la distinción con `00_panorama/BITACORA.md`.

---

## Templates que NO existen

- `00_panorama/stakeholders.md` — generar solo si la matriz RACI es grande.
- `10_producto/personas/` — generar solo si las audiencias son complejas.
- `10_producto/wireframes/navigation.html` — generar solo si hay UX/UI.
- `20_especificacion_tecnica/deployment/production.md` — generar solo si producción está definida.

Cuando un agente decide generar uno de estos, **debe escalar al usuario** primero para validar que el archivo es necesario.

---

End of templates index.
