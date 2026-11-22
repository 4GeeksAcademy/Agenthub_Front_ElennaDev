# Análisis: spec.md frente al diseño de Stitch, y plan para index.html

## Context
Partimos de `specs/agenthub-dashboard/spec.md`, que es la fuente de verdad. Stitch ha generado 9 pantallas estáticas en `stitch_preference/`, junto con dos guías de estilo: `DESIGN.md` (oscuro) y `DESIGN_LIGHT.md` (claro). El objetivo es reutilizar el HTML de Stitch para construir un único `index.html` que cumpla la spec. Este documento analiza las diferencias y propone los pasos. No se ha modificado ningún archivo.

Decisión ya tomada: **quitar Google Fonts** y usar las fuentes del sistema (sans y mono), manteniendo los tamaños, pesos y tracking de DESIGN.md.

## Resumen rápido de restricciones (las 9 pantallas)
| Comprobación | Resultado |
|---|---|
| Bloques `<style>` | 0 ✅ |
| Atributos `style` | 0 ✅ |
| Iconos | SVG inline, sin Material Symbols ✅ |
| Google Fonts (`<link>` a fonts.googleapis.com) | ❌ presente en las 9 → eliminar |
| Tailwind CDN con `?plugins=forms,container-queries` | ⚠️ `container-queries` no se usa → quitar. `forms` es opcional (normaliza el textarea); se recomienda quitarlo y dar estilo a mano |
| `tailwind.config` con tokens propios (colores, radios, fuentes, tamaños) | ✅ es configuración, no CSS. Se mantiene |
| `fontFamily` en la config apunta a Mulish, Instrument Sans y Spline Sans Mono | ⚠️ cambiar a las pilas del sistema |
| Atributos `data-stitch-vh`, `data-component-*`, `data-design-text-token` | Ruido de Stitch → eliminar |
| `min-h-[1024px]` y `max-h-[921px]` (alturas fijas de la captura) | Sustituir por `min-h-screen` y `max-h-[90vh]` |
| `DESIGN.md` remite a `components/*.html` | No existe la carpeta → ignorar |

## Problema transversal: coherencia del modo claro
La config solo define los tokens `accent-*-soft` y `accent-*-text` con valores **oscuros** (por ejemplo, `accent-green-soft: #14532D`), y las pantallas los usan sin variante `dark:`. En modo claro, los badges, los iconos de las tarjetas y el callout se ven como píldoras oscuras sobre fondo blanco.

Corrección: añadir a la config los tokens `accent-{green,red,yellow,orange,blue,purple,neutral}-light-soft/-light-text` y `brand-indigo-light-border` (valores de DESIGN_LIGHT.md). Luego usar siempre el par claro/oscuro, por ejemplo:
`bg-accent-green-light-soft text-accent-green-light-text dark:bg-accent-green-soft dark:text-accent-green-text`.

Afecta a:
- todos los badges,
- los cuadros de icono del Dashboard,
- el borde del callout,
- el borde del botón de skills cuando está expandido (`border-brand-indigo-border`).

Otros ajustes de modo:
- El backdrop del modal tiene un 60 % de opacidad en oscuro y un 40 % en claro (`bg-black/40 dark:bg-black/60`).

## Layout global
**Reutilizable**
- `tailwind.config` completo: colores, radios, espaciados, tamaños de texto y duraciones (`duration-state-change`, `duration-drawer-toggle`).
- El sidebar completo: logo SVG, 6 enlaces con iconos SVG correctos y el estilo del ítem activo (fondo índigo suave, barra izquierda y `aria-current`).
- La barra superior `sticky` con `h1` y el toggle con los iconos sol/luna (`hidden dark:block` y `block dark:hidden`), que ya funcionan solos.
- El script del toggle (`classList.toggle('dark')` + `aria-pressed`).
- El patrón de tabla: contenedor `overflow-visible`, `thead` con `uppercase label-md`, filas con `divide-y` y hover.

**Falta**
- Unir las 9 páginas en **un solo `index.html`** con 6 `section` (5 ocultas).
- Que el sidebar sea realmente fijo: hoy es una columna flex que se desplaza con la página. Debe ser `fixed inset-y-0`, y `main` debe llevar `ml-56 lg:ml-64`.
- Que los enlaces del sidebar sean `button` (según la spec), no `<a href="#">`.
- Un modal genérico reutilizable y un modal de confirmación (solo existe un modal estático de usuario).
- Un dropdown genérico (solo existe uno estático en Contrataciones).
- Mensajes de tabla vacía.

**Interacciones a implementar**
- Navegación: mostrar/ocultar secciones, cambiar el título y mover el indicador activo.
- Dropdown: abrir y cerrar, uno solo abierto a la vez, cierre con clic fuera, Escape o al elegir una opción, y apertura hacia arriba en las 2 últimas filas.
- Modal: cerrar con ✕, backdrop o Escape; bloquear el scroll del `body`; enfocar al abrir y devolver el foco al cerrar.
- Confirmación genérica con la acción `onConfirm`.

**Ajustes visuales**
- Quitar las alturas fijas.
- Verificar a 768 px: sidebar de 224 px + tabla de Contrataciones de 7 columnas en 544 px. Si no cabe, reducir el padding horizontal de las celdas en `md` (`px-3`) y permitir que los nombres partan línea.

## 1. Dashboard (`dashboard.html`)
- **Reutilizable:** casi todo. La rejilla `grid-cols-1 md:grid-cols-2 gap-6`, las 4 tarjetas con la franja izquierda de color, `shadow-sm`, los iconos (billete, cupón, check, triángulo) y el placeholder con borde discontinuo y etiqueta centrada.
- **Falta:** nada estructural.
- **Interacciones:** "Agentes activos" y "Agentes fallando" deben calcularse desde el array de agentes en cada render (hoy son los literales 3 y 2).
- **Ajustes visuales:**
  - El icono de la tarjeta morada usa `text-brand-indigo-text`. Debe ser morado: añadir `accent-purple-text` y `accent-purple-light-text`.
  - Los cuadros de icono necesitan sus variantes claras.
  - El placeholder mide `min-h-[256px]`; la spec pide unos 256 px, así que vale como está.
  - En la spec la cuadrícula 2×2 empieza en `sm`; Stitch la empieza en `md`. Cualquiera de las dos cumple a 768 px; se mantiene `md`.
- **Datos:** coinciden con §2.4 (331,00 €, 48,00 €, 3 y 2).

## 2. Gestión de usuarios (`gestion_usuarios.html`, `gestion_usuarios_modal.html`)
- **Reutilizable:**
  - La tabla completa: 6 filas con columnas Nombre, Email (`break-all`), Plan en texto plano, Estado con badge y ⋮ con `aria-label`.
  - El modal de detalle: estructura `dl` con todos los campos de U3, cabecera con título y ✕, y cierre por ✕ y backdrop.
- **Falta:**
  - El menú del dropdown (solo existe el botón ⋮).
  - La opción "Eliminar" en rojo.
  - El modal de confirmación.
  - El estado de tabla vacía.
- **Interacciones:** dropdown → "Ver detalle" (modal rellenado desde los datos) y "Eliminar" (confirmación y borrado de la fila).
- **Ajustes visuales:**
  - Badges con su variante clara.
  - El modal usa `max-h-[921px]` → cambiar a `max-h-[90vh]`, y el backdrop a `bg-black/40 dark:bg-black/60`.
  - Los `role="status"` de los badges sobran: anuncian cambios a los lectores de pantalla. Quitarlos.
- **Datos:** coinciden con §2.4 (6 usuarios; el modal de Laura Méndez es correcto).

## 3. Gestión de agentes (`gestion_agentes.html`, `gestion_agentes_skills_exp.html`)
- **Reutilizable:**
  - La tabla: Nombre, Propietario, Estado con badges verde, rojo y gris, y el botón "N skills" con chevron.
  - El estilo del botón expandido (fondo índigo suave).
  - Los chips de la sub-fila (`rounded-sm`, con borde) y la etiqueta "Skills equipadas:".
  - El chevron con `transition-transform duration-drawer-toggle` y `rotate-180`.
- **Falta:**
  - Que la sub-fila exista siempre bajo cada agente. Hoy solo se dibuja la expandida, sin envoltorio animable.
  - El menú del dropdown (Configurar / Eliminar).
  - El modal "Configurar: {nombre}" con el textarea y los botones Guardar/Cancelar.
  - La confirmación de borrado.
- **Interacciones:**
  - Colapsable: alternar entre `max-h-0 opacity-0` y `max-h-40 opacity-100` con `transition-all duration-drawer-toggle`, junto con `rotate-180`, `aria-expanded` y el estilo del botón expandido. Con la sub-fila colapsada, el `td` va sin padding ni borde.
  - Guardar el prompt en el dato y cerrar; Cancelar descarta.
  - Eliminar con confirmación, y recalcular el Dashboard y los badges de Skills.
- **Ajustes visuales:**
  - Variantes claras de los badges y del borde `brand-indigo-border` del botón expandido.
  - El fondo de la fila expandida (`bg-light-surface-container-low`) se mantiene.
- **Datos:** coinciden (6 agentes, propietarios, estados y nº de skills). Faltan los **prompts de sistema**, que no aparecen en ninguna pantalla y hay que tomarlos de §2.4.

## 4. Skills (`skills.html`)
- **Reutilizable:**
  - El callout con su texto exacto de S1.
  - El grid `grid-cols-1 md:grid-cols-2 lg:grid-cols-3`.
  - Las tarjetas `article` con el nombre (`h2`) y el ⋮ en la cabecera (arriba a la derecha), la descripción breve y el badge azul "N agentes".
- **Falta:**
  - El menú del dropdown (Ver detalle / Eliminar).
  - El modal de detalle (descripción amplia, "Agentes que la usan" y precio).
  - La confirmación de borrado.
  - El mensaje de catálogo vacío.
- **Interacciones:**
  - El badge "N agentes" se calcula desde los agentes visibles. Hoy es un literal.
  - Eliminar quita la tarjeta.
- **Ajustes visuales:**
  - El callout usa `role="alert"`, que interrumpe a los lectores de pantalla. Cambiar a `aside role="note"`.
  - El borde del callout y el badge azul necesitan sus variantes claras.
  - El ⋮ está dentro del flujo de la cabecera, no posicionado de forma absoluta. Visualmente cumple "esquina superior derecha", así que se mantiene.
- **Datos:** nombres, descripciones breves y recuentos coinciden. Faltan la **descripción amplia** y el **precio**, que no aparecen en el diseño y hay que tomarlos de §2.4.

## 5. Contrataciones (`contratacion_agentes.html`, `Contratacion_agentes_dropdown.html`)
- **Reutilizable:**
  - La tabla de 7 columnas: Inicio / Fin apilados, sin fecha de fin en los activos; importe `text-right tabular-nums`.
  - El dropdown abierto: el contenedor `relative`, el menú `absolute right-0 w-44 shadow-xl z-30` y `aria-haspopup`, `aria-expanded` y `aria-controls`. Es la **base del dropdown genérico**.
- **Falta:**
  - El estado deshabilitado de "Finalizar contrato" en los contratos finalizados.
  - El modal de detalle con el desglose (Skill / Precio/mes / Meses / Subtotal + Total).
  - La confirmación de finalizar.
- **Interacciones:**
  - Ver detalle: el título "{Agente} — {Cliente}" y el desglose calculado a partir de los precios y los meses.
  - Finalizar: badge "Finalizado", fecha de fin igual a hoy en DD/MM/YY y opción deshabilitada.
  - El importe se calcula (precio × meses), no es un literal.
- **Ajustes visuales:**
  - Las opciones del menú son `<li role="menuitem">` con `cursor-pointer`. Deben ser `<button role="menuitem">` dentro de `li role="none"`, para que funcionen con teclado y admitan `disabled`.
  - La opción deshabilitada lleva `opacity-50 cursor-not-allowed`.
  - Badges con su variante clara.
- **Datos:** los 7 contratos, fechas, estados, nº de skills e importes coinciden con §2.4. El desglose por skill no está en el diseño y sale del dataset.

## 6. Log de errores (`log_errores.html`)
- **Reutilizable:** la tabla con las columnas Fecha y hora, Agente, Gravedad (badges rojo, naranja y amarillo) y Descripción, y la columna de acciones `sr-only`. Los tokens `terminal-bg` y `terminal-text` ya están en la config.
- **Falta:**
  - El menú del dropdown (Ver detalle / Marcar como resuelto).
  - El modal de detalle con la traza en `pre font-mono bg-terminal-bg text-terminal-text`, que es oscuro en ambos modos.
  - La confirmación.
  - El mensaje "No hay errores pendientes.".
- **Interacciones:**
  - Ordenar por timestamp descendente en JS antes de dibujar.
  - Marcar como resuelto elimina la entrada.
- **Ajustes visuales:** variantes claras de los badges de gravedad.
- **Datos:** las 7 entradas coinciden (fechas, agentes, gravedad y descripción). Faltan las **trazas técnicas** y el **nombre del error** como título del modal (hoy solo aparece en el `aria-label`), que salen de §2.4.

## Plan de implementación (siguiente paso, tras aprobar)
1. Crear `index.html` en la raíz.
   - `<head>`: Tailwind CDN sin plugins y la config de Stitch con `fontFamily` en pilas del sistema y los tokens `*-light-*` añadidos.
   - Sin Google Fonts.
2. Montar el layout con el sidebar (fijo, con botones) y la barra superior de `dashboard.html`. Debajo, 6 `section`, con el marcado de cada pantalla como plantilla.
3. Dataset JS de §2.4 (empresas, skills, agentes, usuarios, contratos con `meses`, errores con traza y métricas), con resolución de nombres por id y la marca de eliminado según §2.5.
4. Funciones de render por sección, que generan las filas y las tarjetas desde los datos usando las clases de Stitch. Texto del usuario siempre con `textContent`.
5. Componentes:
   - dropdown genérico (basado en el de Contrataciones),
   - modal y confirmación genéricos (basados en el modal de usuario),
   - colapsable de skills,
   - navegación,
   - toggle de modo.
6. Pasada de modo claro: sustituir cada `accent-*-soft/text` por su par claro/oscuro.
7. Limpieza: quitar los `data-*` de Stitch, `role="status"` y `role="alert"`, y las alturas fijas.

## Verificación
- Abrir `index.html` en el navegador y recorrer los 46 criterios de §5 de la spec.
- En la consola: `document.querySelectorAll('[style]').length === 0` tras interactuar con todo.
- Comprobar que no se carga `fonts.googleapis.com` (pestaña Red: solo `cdn.tailwindcss.com`).
- Probar a 768 px y a 1280 px (sin scroll horizontal) y en ambos modos (badges legibles en claro).
