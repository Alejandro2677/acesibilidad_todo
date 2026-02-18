# WCAG 2.2 Prompt Lab (ES/EN)

## ES - Resumen

Repositorio de practica para crear y probar un prompt "unico y robusto" que permita a asistentes de programacion (Copilot, Gemini, etc.) transformar HTML no accesible en HTML alineado con WCAG 2.2.

Incluye:
- Archivos originales (sin accesibilidad).
- Archivos corregidos (A/AA + mejoras AAA razonables).
- Prompt maestro reutilizable.
- Estructura para evidencias de validacion (capturas before/after).

## ES - Asistentes usados

- GitHub Copilot (VS Code)
- Gemini (Antigravity / web)
- Cualquier asistente compatible con prompts largos y salida estructurada

Prompt de referencia:
- `prompts/prompt-perfecto-wcag22.md`

## ES - Estructura

- `src/original/index-form.html`
- `src/original/index-table.html`
- `src/original/index-landing.html`
- `src/fixed/index-form-accessible.html`
- `src/fixed/index-table-accessible.html`
- `src/fixed/index-landing-accessible.html`
- `prompts/prompt-perfecto-wcag22.md`
- `docs/validation/before/`
- `docs/validation/after/`
- `docs/validation/validation-log-template.md`

## ES - Proceso de desarrollo del prompt

1. Se definieron fallos frecuentes en HTML real:
   - Falta de semantica, labels, alt, contraste y foco visible.
   - Jerarquia de encabezados inconsistente.
   - Tablas sin `caption` ni `th` con `scope`.
2. Se redacto un prompt con:
   - Objetivo tecnico claro (WCAG 2.2 A/AA + mejoras AAA).
   - Reglas obligatorias por componente (formularios, tablas, imagenes, enlaces, teclado).
   - Formato de salida auditable (HTML + mapeo WCAG + checklist de validacion).
3. Se aplico el prompt sobre cada HTML original para obtener una version corregida.
4. Se preparo un flujo de validacion repetible con multiples herramientas.

## ES - Pasos de validacion de accesibilidad

Herramientas objetivo:
- WAVE
- Axe DevTools
- Lighthouse
- Accessibility Insights
- Access Assistant
- ANDI
- ARC Toolkit

Procedimiento por archivo:
1. Abrir `src/original/*.html` en navegador y ejecutar herramientas.
2. Guardar capturas en `docs/validation/before/`.
3. Abrir `src/fixed/*.html` y repetir validacion.
4. Guardar capturas en `docs/validation/after/`.
5. Registrar incidencias y estado final en `docs/validation/validation-log-template.md`.
6. Si hay errores, volver al HTML original, mejorar el prompt (no encadenar prompts parciales), regenerar y validar de nuevo.

## ES - Problemas encontrados y soluciones aplicadas

- Problema: Inputs sin etiqueta y placeholders como unico texto.
  Solucion: `label for`, ayudas con `aria-describedby`, `fieldset/legend` para grupos.
- Problema: Enlaces ambiguos ("click aqui", "mas").
  Solucion: texto de enlace descriptivo por contexto.
- Problema: Tabla sin cabeceras reales.
  Solucion: `caption`, `thead/tbody`, `th scope="col|row"`.
- Problema: Navegacion por teclado deficiente.
  Solucion: skip link, `:focus-visible`, sin `tabindex` positivo.
- Problema: Contraste insuficiente.
  Solucion: paleta con contraste AA y mejoras cercanas a AAA para texto normal.

## ES - Enlaces a capturas (rellenar con tus evidencias)

Before:
- `docs/validation/before/wave-index-form-before.png`
- `docs/validation/before/axe-index-form-before.png`
- `docs/validation/before/lighthouse-index-form-before.png`

After:
- `docs/validation/after/wave-index-form-after.png`
- `docs/validation/after/axe-index-form-after.png`
- `docs/validation/after/lighthouse-index-form-after.png`

Repetir el patron para `index-table` e `index-landing`.

---

## EN - Summary

Practice repository to design and test a "single robust prompt" that guides coding assistants (Copilot, Gemini, etc.) to transform non-accessible HTML into WCAG 2.2-aligned HTML.

Includes:
- Original files (non-accessible).
- Fixed files (A/AA + reasonable AAA improvements).
- Reusable master prompt.
- Validation evidence structure (before/after screenshots).

## EN - Assistants used

- GitHub Copilot (VS Code)
- Gemini (Antigravity / web)
- Any assistant that supports long prompts and structured output

Reference prompt:
- `prompts/prompt-perfecto-wcag22.md`

## EN - Prompt development process

1. Identify recurring accessibility issues in baseline HTML.
2. Build one strict prompt with mandatory rules:
   - semantic HTML5 landmarks
   - form labels and ARIA relationships
   - image alternatives
   - table semantics
   - keyboard navigation and visible focus
   - color contrast checks
3. Run the prompt on each original file.
4. Validate with multiple tools and iterate from the original source if needed.

## EN - Validation workflow

Target tools:
- WAVE
- Axe DevTools
- Lighthouse
- Accessibility Insights
- Access Assistant
- ANDI
- ARC Toolkit

Per file:
1. Validate `src/original/*.html`.
2. Save screenshots in `docs/validation/before/`.
3. Validate `src/fixed/*.html`.
4. Save screenshots in `docs/validation/after/`.
5. Document all results in `docs/validation/validation-log-template.md`.
6. If issues remain, refine the prompt and re-run from original HTML (single-prompt iteration).

## EN - Notes

AAA conformance is optional/conditional in many real projects; this repository targets full A/AA and applies AAA-oriented improvements where feasible.

## EN - Screenshot links (fill with your evidence)

Before:
- `docs/validation/before/wave-index-form-before.png`
- `docs/validation/before/axe-index-form-before.png`
- `docs/validation/before/lighthouse-index-form-before.png`

After:
- `docs/validation/after/wave-index-form-after.png`
- `docs/validation/after/axe-index-form-after.png`
- `docs/validation/after/lighthouse-index-form-after.png`

Repeat the same pattern for `index-table` and `index-landing`.

# Prompt maestro 

Actua como especialista senior en accesibilidad web (WCAG 2.2 + WAI-ARIA + HTML semantico).

Objetivo:
Reescribe el HTML que te doy para cumplir criterios WCAG 2.2 nivel A y AA, e incluir mejoras razonables de AAA cuando apliquen sin cambiar la funcionalidad del sitio.

Entrada:
- Un archivo HTML con problemas de accesibilidad.

Salida obligatoria (en este orden):
1) Codigo HTML completo corregido.
2) Lista de cambios aplicada, mapeada por criterio WCAG (ej: 1.1.1, 1.3.1, 1.4.3, 2.1.1, 2.4.3, 2.4.6, 2.4.7, 3.3.2, 4.1.2, etc.).
3) Checklist final "pass/fail" de validacion esperada para WAVE, Axe y Lighthouse.
4) Riesgos no verificables automaticamente (ej: calidad real de alt, claridad de textos, lenguaje claro).

Reglas tecnicas:
- Mantener DOCTYPE y agregar <html lang="..."> correcto.
- Usar estructura semantica real: <header>, <nav>, <main>, <section>, <article>, <aside>, <footer> segun corresponda.
- Mantener una jerarquia de encabezados valida (h1 unico principal y subsecciones ordenadas).
- Formularios:
  - Cada control debe tener <label for>.
  - Asociar ayudas y errores con aria-describedby.
  - Usar fieldset/legend cuando existan grupos de opciones.
  - No usar placeholder como unica etiqueta.
- Imagenes:
  - Alt descriptivo para contenido relevante.
  - alt="" y aria-hidden="true" para decorativas.
- Enlaces y botones:
  - Nombre accesible claro y unico por contexto.
  - Evitar textos vagos (ej: "click aqui", "leer mas") sin contexto.
- Tablas:
  - caption obligatorio.
  - th y scope correctos.
- Teclado:
  - Navegacion completa sin raton.
  - No usar tabindex positivo.
  - Agregar foco visible con :focus-visible.
  - Incluir skip link al contenido principal.
- ARIA:
  - Usar ARIA solo cuando HTML nativo no resuelva el problema.
  - No duplicar semantica nativa.
- Contraste:
  - Asegurar minimo AA (4.5:1 texto normal, 3:1 texto grande y UI).
  - Aplicar mejora AAA en texto normal (7:1) cuando sea posible.
- Idioma y legibilidad:
  - Definir idioma del documento y cambios de idioma puntuales.
  - Evitar bloques de texto justificado y estilos que perjudiquen lectura.
- accesskey:
  - Evitarlo por defecto por conflictos de teclado.
  - Solo usar si se documenta la combinacion y no rompe atajos del navegador/SO.

Restricciones:
- No eliminar contenido funcional.
- No introducir JS innecesario.
- Mantener compatibilidad con lectores de pantalla y validadores automaticos.
- Si falta informacion para una decision (por ejemplo, alt exacto), deja una propuesta contextual y marcalo en "Riesgos no verificables automaticamente".

# Master prompt (Copilot / Gemini / other assistants)

Use this prompt in a single run on an original HTML (not accessible) to obtain a corrected and traceable version.

Act as a senior web accessibility specialist (WCAG 2.2 + WAI-ARIA + semantic HTML).

Objective:
Rewrite the HTML I provide to meet WCAG 2.2 Level A and AA criteria, and include reasonable AAA improvements where applicable without changing the site’s functionality.

Input:

One HTML file with accessibility issues.

Required output (in this order):

Complete corrected HTML code.

List of changes applied, mapped by WCAG criterion (e.g., 1.1.1, 1.3.1, 1.4.3, 2.1.1, 2.4.3, 2.4.6, 2.4.7, 3.3.2, 4.1.2, etc.).

Final “pass/fail” checklist of expected validation results for WAVE, Axe, and Lighthouse.

Non-automatically verifiable risks (e.g., real alt quality, clarity of texts, plain language).

Technical rules:

Keep the DOCTYPE and add the correct <html lang="...">.

Use a real semantic structure: <header>, <nav>, <main>, <section>, <article>, <aside>, <footer> as appropriate.

Maintain a valid heading hierarchy (a single main h1, and ordered subsections).

Forms:

Every control must have a <label for>.

Associate help text and errors with aria-describedby.

Use fieldset/legend when there are groups of options.

Do not use placeholder as the only label.

Images:

Descriptive alt text for relevant content.

alt="" and aria-hidden="true" for decorative images.

Links and buttons:

Clear, unique accessible name per context.

Avoid vague texts (e.g., “click here”, “read more”) without context.

Tables:

Caption is required.

Correct th and scope.

Keyboard:

Full navigation without a mouse.

Do not use positive tabindex.

Add visible focus with :focus-visible.

Include a skip link to the main content.

ARIA:

Use ARIA only when native HTML doesn’t solve the problem.

Do not duplicate native semantics.

Contrast:

Ensure at least AA (4.5:1 for normal text, 3:1 for large text and UI).

Apply AAA improvement for normal text (7:1) when possible.

Language and readability:

Define the document language and any inline language changes.

Avoid justified text blocks and styles that harm readability.

accesskey:

Avoid it by default due to keyboard conflicts.

Only use it if the key combination is documented and doesn’t break browser/OS shortcuts.

Restrictions:

Do not remove functional content.

Do not introduce unnecessary JS.

Maintain compatibility with screen readers and automatic validators.

If information is missing for a decision (e.g., exact alt text), provide a contextual proposal and flag it under “Non-automatically verifiable risks”.
