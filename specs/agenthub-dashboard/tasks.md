# AgentHub Admin — Tareas de implementación

Referencias:
- [spec.md](spec.md) es la fuente de verdad.
- [plan.md](plan.md) contiene el análisis frente a Stitch.
- `stitch_preference/` contiene el HTML de referencia.
- `DESIGN.md` y `DESIGN_LIGHT.md` contienen los tokens visuales.

## Principios
- Prioriza reutilizar la estructura y los estilos de `stitch_preference/` cuando cumplan los requisitos. Si hay conflicto entre Stitch y `spec.md`, manda `spec.md`.
- No utilices frameworks adicionales ni dependencias: solo Tailwind CDN y JavaScript vanilla.
- Todo el trabajo se hace en un único `index.html` en la raíz. No hay archivos CSS, ni bloques `<style>`, ni atributos `style` (tampoco asignados desde JS).
- Cada tarea se marca `[x]` solo cuando su verificación pasa.

---

## 1. Base: `index.html`, head y layout
- [x] **1.1** Crear `index.html` en la raíz con `<!DOCTYPE html>`, `<html lang="es" class="dark">`, `meta charset`, `viewport` y `<title>AgentHub Admin</title>`.
  - *Verifica:* el archivo abre en el navegador sin errores en consola.
- [x] **1.2** Cargar Tailwind con `https://cdn.tailwindcss.com`, sin `?plugins=`.
  - *Verifica:* en la pestaña Red solo aparece `cdn.tailwindcss.com` como recurso externo.
- [x] **1.3** Copiar la `tailwind.config` de `stitch_preference/dashboard.html` (colores, radios, espaciados, tamaños de texto y duraciones) con `darkMode: 'class'`.
  - *Verifica:* una clase de prueba como `bg-dark-surface` aplica `#1B1B24`.
- [x] **1.4** Cambiar `fontFamily` en la config a pilas del sistema: `display`, `headline`, `title`, `body` y `label` → `ui-sans-serif, system-ui, sans-serif`; `mono` → `ui-monospace, SFMono-Regular, Menlo, monospace`. No añadir `<link>` a Google Fonts.
  - *Verifica:* el código fuente no contiene `fonts.googleapis.com` y el texto se renderiza en la fuente del sistema.
- [x] **1.5** Añadir a la config los tokens claros de `DESIGN_LIGHT.md`:
  - `accent-{green,red,yellow,orange,blue,neutral}-light-soft` y `-light-text`,
  - `accent-purple-light-soft`, `accent-purple-light-text` y `accent-purple-text` (oscuro),
  - `brand-indigo-light-border`.
  - *Verifica:* `bg-accent-green-light-soft` aplica `#DCFCE7` y `text-accent-purple-light-text` aplica `#6B21A8`.
- [x] **1.6** `body` con fondo y texto en claro y oscuro (`bg-light-background dark:bg-dark-background`, `text-light-on-background dark:text-dark-on-background`, `font-body antialiased`).
  - *Verifica:* el fondo cambia entre `#F8FAFC` y `#0E0D16` al quitar o poner `dark` en `<html>`.
- [x] **1.7** Sidebar: `aside` con `fixed inset-y-0 left-0 w-56 lg:w-64` y el logo y los 6 ítems de `dashboard.html`. Los ítems son `<button type="button" data-section="…">` dentro de `nav > ul > li`, no enlaces `<a>`.
  - *Verifica:* al hacer scroll largo el sidebar no se mueve; los 6 ítems muestran icono y nombre.
- [x] **1.8** Contenedor principal con `ml-56 lg:ml-64 min-h-screen` y barra superior `header sticky top-0` con el `h1` y el toggle (marcado de Stitch, todavía sin lógica).
  - *Verifica:* la barra queda fija arriba al hacer scroll y no se solapa con el sidebar.
- [x] **1.9** Dentro de `main`, 6 `section` (`sec-dashboard`, `sec-usuarios`, `sec-agentes`, `sec-skills`, `sec-contratos` y `sec-errores`) con contenedores vacíos para las tablas y el grid. Todas, salvo Dashboard, llevan `hidden`.
  - *Verifica:* `document.querySelectorAll('main > section').length === 6` y solo Dashboard es visible.
- [x] **1.10** Contenedores de tabla basados en Stitch: `rounded-lg`, borde, `overflow-visible` y `thead` con cabeceras `uppercase`. Columnas:
  | Sección | Columnas |
  |---|---|
  | Usuarios | Nombre, Email, Plan, Estado, Acciones (`sr-only`) |
  | Agentes | Nombre, Propietario, Estado, Skills, Acciones (`sr-only`) |
  | Contrataciones | Cliente, Agente, Skills, Inicio / Fin, Estado, Importe, Acciones (`sr-only`) |
  | Log de errores | Fecha y hora, Agente, Gravedad, Descripción, Acciones (`sr-only`) |
  - *Verifica:* cada tabla muestra sus cabeceras según spec §3.
- [x] **1.11** Un único contenedor de modal al final de `body`, oculto con `hidden`: backdrop `fixed inset-0 z-50 bg-black/40 dark:bg-black/60` y panel `role="dialog" aria-modal="true"` con `max-w-lg max-h-[90vh] overflow-y-auto`, cabecera (título y ✕), cuerpo y pie.
  - *Verifica:* al quitar `hidden` a mano se ve el panel centrado y vacío.

## 2. Dataset JS y render de las 6 secciones
- [x] **2.1** Añadir `<script>` al final de `body` con el dataset de spec §2.4: `empresas`, `skills` (precio, descripción breve y amplia), `agentes` (empresaId, estado, skillIds, prompt), `usuarios` (empresaId, agenteIds, alta), `contratos` (empresaId, agenteId, skillIds, inicio, fin, estado, meses), `errores` (timestamp, agenteId, gravedad, nombre, descripción, traza) y `metricas`.
  - *Verifica:* en consola, `agentes.length === 6`, `contratos.length === 7` y `errores.length === 7`.
- [x] **2.2** Helpers de búsqueda por id (empresa, agente, skill) y de formato: euros `es-ES` (`540,00 €`), fecha `DD/MM/YY` y fecha y hora `DD/MM/YY HH:mm`.
  - *Verifica:* en consola, `fmtEuro(540)` devuelve `"540,00 €"` y la fecha `2026-03-01` se formatea como `"01/03/26"`.
- [x] **2.3** Helpers derivados:
  - importe de un contrato = Σ precio de sus skills × meses,
  - agentes visibles (no eliminados),
  - nº de agentes por skill,
  - recuento de agentes por estado.
  - *Verifica:* el importe de c3 es 890; los recuentos son activos 3 y fallando 2; Navegación web 3, Gestión de calendarios 1 e Integración CRM 2.
- [x] **2.4** Helper de badge que devuelve el par claro/oscuro por color (verde, gris, rojo, amarillo, naranja, azul), con la píldora de Stitch y sin `role="status"`.
  - *Verifica:* un badge verde lleva `bg-accent-green-light-soft` y `dark:bg-accent-green-soft`.
- [x] **2.5** Render del Dashboard: 4 tarjetas de `dashboard.html` en `grid-cols-1 md:grid-cols-2`. Ingresos y pérdidas salen de `metricas`; activos y fallando, de los recuentos. Debajo, el placeholder del gráfico. El icono de la tarjeta de pérdidas va en morado, no en índigo.
  - *Verifica:* se ven 331,00 €, 48,00 €, 3 y 2 con acentos azul, morado, verde y rojo.
- [x] **2.6** Render de Usuarios: filas con el marcado de `gestion_usuarios.html`. Plan en texto plano, estado con badge y botón ⋮ con `aria-label="Acciones de {nombre}"`.
  - *Verifica:* 6 filas; Marta Iglesias y Pablo Serrano en gris "Inactivo".
- [x] **2.7** Render de Agentes: filas de `gestion_agentes.html` (propietario resuelto por empresaId y badges Activo, Inactivo o Fallando) y, bajo cada fila, la sub-fila de skills colapsada con sus chips.
  - *Verifica:* 6 filas y 6 sub-filas; Orion Ventas y Vega Finanzas en rojo; el botón dice "3 skills" o "1 skill".
- [x] **2.8** Render de Skills: callout `aside role="note"` con el texto de spec S1 y grid `grid-cols-1 md:grid-cols-2 lg:grid-cols-3` con las tarjetas de `skills.html` (nombre, ⋮, descripción breve y badge azul "N agentes" calculado).
  - *Verifica:* 6 tarjetas con los recuentos 3, 3, 1, 3, 3 y 2.
- [x] **2.9** Render de Contrataciones: filas de `contratacion_agentes.html`. Inicio y fin apilados (sin fin en los activos), badge Activo o Finalizado y el importe calculado `text-right tabular-nums`.
  - *Verifica:* 7 filas con importes 540, 189, 890, 594, 114, 606 y 576 €.
- [x] **2.10** Render del Log: ordenar las entradas por timestamp descendente antes de dibujarlas. Filas de `log_errores.html` con el badge de gravedad (Leve amarillo, Medio naranja, Grave rojo).
  - *Verifica:* la primera fila es 21/11/26 09:42 (Orion Ventas) y la última 17/11/26 14:02.
- [x] **2.11** Mensajes de lista vacía (spec §2.5.6) en las 4 tablas y en el catálogo.
  - *Verifica:* al vaciar a mano un array y volver a dibujar aparece el mensaje correspondiente.
- [x] **2.12** Todo texto que proceda del dataset (sobre todo el prompt editable) se inserta con `textContent` o `.value`, nunca con `innerHTML`.
  - *Verifica:* con el prompt `<b>x</b>`, el modal lo muestra literal, sin negrita.

## 3. Navegación entre secciones
- [x] **3.1** Al hacer clic en un ítem del sidebar se muestra su `section` y se ocultan las otras 5.
  - *Verifica:* al pulsar cada ítem solo es visible su sección.
- [x] **3.2** El `h1` de la barra superior se actualiza con el título de la sección: Dashboard, Gestión de usuarios, Gestión de agentes, Skills, Contrataciones de agentes o Log de errores.
  - *Verifica:* el título coincide con la spec en las 6 secciones.
- [x] **3.3** Indicador del ítem activo con las clases de Stitch (fondo índigo suave, texto índigo, `border-l-2`, negrita) y `aria-current="page"` solo en el activo. Los demás llevan el estilo de hover de Stitch.
  - *Verifica:* `document.querySelectorAll('[aria-current="page"]').length === 1` después de cada clic.
- [x] **3.4** Al navegar a una sección se vuelve a dibujar (para reflejar los datos derivados) y se cierra cualquier dropdown abierto.
  - *Verifica:* tras eliminar un agente activo, Dashboard muestra 2 activos.

## 4. Dropdowns
- [x] **4.1** Componente dropdown basado en `Contratacion_agentes_dropdown.html`: un wrapper `relative inline-block`, un botón ⋮ con `aria-haspopup="menu"` y `aria-expanded`, y un menú `ul role="menu"` `absolute right-0 w-44 shadow-xl z-30` con `hidden`.
  - *Verifica:* cada fila y cada tarjeta tiene su menú oculto en el DOM.
- [x] **4.2** Opciones como `<button type="button" role="menuitem">` dentro de `li role="none"`, no como `li` clicable. Las destructivas ("Eliminar") van en rojo.
  - *Verifica:* las opciones se alcanzan con Tab y "Eliminar" se ve roja en ambos modos.
- [x] **4.3** Opciones por sección:
  | Sección | Opciones |
  |---|---|
  | Usuarios | Ver detalle, Eliminar |
  | Agentes | Configurar, Eliminar |
  | Skills | Ver detalle, Eliminar |
  | Contrataciones | Ver detalle, Finalizar contrato |
  | Log | Ver detalle, Marcar como resuelto |
  - *Verifica:* al abrir un ⋮ de cada sección aparecen exactamente esas opciones.
- [x] **4.4** Un clic en ⋮ abre su menú y cierra cualquier otro (solo uno abierto), y actualiza `aria-expanded`.
  - *Verifica:* tras abrir dos menús seguidos, solo el segundo está visible.
- [x] **4.5** Cierre con clic fuera (un solo listener en `document` con `closest`), con Escape y al elegir una opción.
  - *Verifica:* las tres formas cierran el menú.
- [x] **4.6** En las 2 últimas filas de cada tabla, el menú se abre hacia arriba (`bottom-full mb-1` en lugar de `top-full mt-1`).
  - *Verifica:* el menú de la última fila del Log se ve completo, sin recortarse.
- [x] **4.7** En los contratos finalizados, "Finalizar contrato" lleva `disabled`, `aria-disabled="true"` y `opacity-50 cursor-not-allowed`.
  - *Verifica:* en c5 y c7 la opción está atenuada y el clic no hace nada.

## 5. Modales y modal de confirmación
- [x] **5.1** Funciones para abrir el modal (título, cuerpo y pie opcional) y cerrarlo, usando el contenedor de la tarea 1.11. Al abrir, `overflow-hidden` en `body` y foco en ✕; al cerrar, el foco vuelve al botón que lo abrió.
  - *Verifica:* tras abrir y cerrar, el foco vuelve al ⋮ y la página vuelve a hacer scroll.
- [x] **5.2** Cierre con ✕, con clic en el backdrop (solo si el clic cae en el propio backdrop) y con Escape.
  - *Verifica:* las tres formas cierran el modal y un clic dentro del panel no lo cierra.
- [x] **5.3** Modal de confirmación sobre el modal genérico: título, mensaje, botón "Cancelar" (secundario) y botón de confirmar (rojo para "Eliminar", índigo para "Finalizar" y "Marcar como resuelto"). Al confirmar se ejecuta la acción, se cierra y se vuelve a dibujar la vista.
  - *Verifica:* si se cancela, no cambia nada.
- [x] **5.4** Detalle de usuario con la `dl` de `gestion_usuarios_modal.html`: Nombre, Email, Plan, Estado (badge), Fecha de alta, Empresa y la lista de Agentes contratados.
  - *Verifica:* en Diego Ramírez aparecen Bravia Retail y la lista "Orion Ventas" y "Nova Marketing".
- [x] **5.5** Eliminar usuario: confirmación "¿Eliminar a {nombre}? Esta acción no se puede deshacer." y, al confirmar, la fila desaparece.
  - *Verifica:* la tabla queda con 5 filas.
- [x] **5.6** Configurar agente: título "Configurar: {nombre}", etiqueta "Prompt de sistema", textarea de 8 filas y botones Cancelar y Guardar. Guardar escribe el texto en el dato; Cancelar, ✕, el backdrop y Escape lo descartan.
  - *Verifica:* tras editar y guardar, al reabrir aparece el texto nuevo; tras editar y cancelar, aparece el anterior.
- [x] **5.7** Eliminar agente: confirmación y, al confirmar, se marca como eliminado; desaparecen la fila y la sub-fila, y se recalculan el Dashboard y Skills.
  - *Verifica:* Skills y Dashboard reflejan el cambio y el agente sigue nombrado en Contrataciones y Log.
- [x] **5.8** Detalle de skill: descripción amplia, "Agentes que la usan" (o "Ningún agente la usa actualmente.") y "Precio" como `29,00 € / mes`.
  - *Verifica:* Gestión de calendarios muestra solo "Clara Agenda" y 15,00 € / mes.
- [x] **5.9** Eliminar skill: confirmación "¿Eliminar la skill {nombre} del catálogo?"; la tarjeta desaparece y su nombre y precio se mantienen en el desglose de los contratos.
  - *Verifica:* el catálogo queda con 5 tarjetas y el detalle de c1 sigue igual.
- [x] **5.10** Detalle de contrato: título "{Agente} — {Cliente}"; `dl` con Cliente, Agente, Inicio, Fin y Estado; tabla Skill / Precio/mes / Meses / Subtotal y "Total" en negrita.
  - *Verifica:* en c1, Navegación web muestra 29,00 € · 9 · 261,00 € y el Total es 540,00 €, igual que en la tabla.
- [x] **5.11** Finalizar contrato: confirmación "¿Finalizar el contrato de {Agente} con {Cliente}?". Al confirmar: estado finalizado, fin = hoy, el modal se cierra y la vista se vuelve a dibujar.
  - *Verifica:* el badge pasa a "Finalizado" gris, la fecha de fin es la de hoy en DD/MM/YY, la opción queda deshabilitada y el importe no cambia.
- [x] **5.12** Detalle de error: título con el nombre del error y, en orden, Agente, Fecha y hora, Gravedad (badge), Descripción y "Traza técnica" en `pre font-mono text-mono-sm bg-terminal-bg text-terminal-text rounded-lg p-4 overflow-x-auto whitespace-pre`.
  - *Verifica:* la traza de r1 aparece en 3 líneas sobre fondo oscuro, también en modo claro.
- [x] **5.13** Marcar como resuelto: confirmación "¿Marcar como resuelto el error {nombre} de {agente}?"; al confirmar, la entrada desaparece.
  - *Verifica:* el log queda con 6 entradas y sigue ordenado de más reciente a más antigua.

## 6. Colapsable de skills
- [x] **6.1** La sub-fila lleva `td colspan="5"` y un `div` envoltorio con `overflow-hidden transition-all duration-drawer-toggle ease-drawer-toggle`, colapsado como `max-h-0 opacity-0` y con el `td` sin padding ni borde.
  - *Verifica:* al cargar no se ve ningún hueco entre las filas de agentes.
- [x] **6.2** El botón "N skills" lleva `aria-expanded="false"` y `aria-controls="skills-{id}"`, y el chevron apunta hacia abajo por defecto.
  - *Verifica:* en el estado inicial los 6 botones tienen `aria-expanded="false"`.
- [x] **6.3** Al hacer clic se alterna `max-h-0 opacity-0` ↔ `max-h-40 opacity-100`, se gira el chevron con `rotate-180`, se actualiza `aria-expanded` y se aplica al botón el estilo expandido de `gestion_agentes_skills_exp.html`.
  - *Verifica:* la sub-fila se abre y se cierra con una transición visible de unos 300 ms y la flecha apunta arriba cuando está abierta.
- [x] **6.4** Cada agente se despliega de forma independiente.
  - *Verifica:* se pueden tener dos agentes desplegados a la vez.

## 7. Modo claro/oscuro
- [x] **7.1** El toggle alterna `dark` en `<html>` y actualiza `aria-pressed`. Los iconos sol/luna cambian con `hidden dark:block` y `block dark:hidden`.
  - *Verifica:* al cargar se ve oscuro con el sol; tras un clic, claro con la luna.
- [x] **7.2** El modo se mantiene al navegar porque la navegación no toca `<html>`.
  - *Verifica:* tras cambiar a claro y recorrer las 6 secciones, sigue en claro.
- [x] **7.3** Pasada de variantes claras en los badges: todos usan el par `*-light-soft/*-light-text` y `dark:*-soft/dark:*-text`.
  - *Verifica:* en modo claro no queda ningún badge con fondo oscuro.
- [x] **7.4** Pasada de variantes claras en los cuadros de icono del Dashboard, en el callout (borde `brand-indigo-light-border` y `dark:border-brand-indigo-border`) y en el botón de skills expandido.
  - *Verifica:* en modo claro los iconos, el callout y el botón expandido se ven suaves y legibles.
- [x] **7.5** Revisar en modo claro el sidebar, la barra superior, las tablas, los menús, los modales y el bloque de traza.
  - *Verifica:* no queda ningún texto ilegible ni superficie oscura en modo claro, salvo el bloque de traza.

## 8. Limpieza
- [x] **8.1** Quitar los atributos de Stitch `data-stitch-vh`, `data-component-id`, `data-component-type`, `data-component-variant`, `data-design-text-token` y `id="tailwind-config"` (este último solo si no se usa).
  - *Verifica:* buscar `data-stitch\|data-component\|data-design` en `index.html` devuelve 0 resultados.
- [x] **8.2** Quitar `role="status"` de los badges y `role="alert"` del callout (este ya es `aside role="note"`).
  - *Verifica:* buscar `role="status"\|role="alert"` devuelve 0 resultados.
- [x] **8.3** Sustituir las alturas fijas de Stitch: `min-h-[1024px]` → `min-h-screen` y `max-h-[921px]` → `max-h-[90vh]`.
  - *Verifica:* buscar `1024px\|921px` devuelve 0 resultados.
- [x] **8.4** Confirmar que no hay `<style>`, ni atributos `style`, ni `.style.` en el JS, ni `<link>` a fuentes.
  - *Verifica:* buscar `<style\|style=\|\.style\.\|fonts.googleapis` devuelve 0 resultados.

## 9. Verificación contra spec.md §5
- [x] **9.1** Criterios 1–5 (estructura, recursos, semántica, idioma y formatos, responsive).
  - *Verifica:* revisión en el navegador a 768 px y a 1280 px; `document.querySelectorAll('[style]').length === 0` tras usar todas las funciones.
- [x] **9.2** Criterios 6–7 (navegación).
  - *Verifica:* recorrido manual de los 6 ítems.
- [x] **9.3** Criterios 8–10 (modo oscuro).
  - *Verifica:* recorrido manual en ambos modos.
- [x] **9.4** Criterios 11–17 (coherencia de datos).
  - *Verifica:* comparar las tablas y los modales con spec §2.4 (empresas, recuentos, totales y agentes del log).
- [x] **9.5** Criterios 18–22 (dropdowns).
  - *Verifica:* probar cada forma de cierre y los menús de las últimas filas.
- [x] **9.6** Criterios 23–30 (modales y configurar).
  - *Verifica:* abrir cada modal y cerrarlo con ✕, con el backdrop y con Escape; guardar y cancelar el prompt.
- [x] **9.7** Criterios 31–37 (confirmaciones y mutaciones).
  - *Verifica:* cancelar y confirmar en cada acción: eliminar, finalizar y resolver.
- [x] **9.8** Criterios 38–40 (colapsable).
  - *Verifica:* estado inicial, desplegar, colapsar y transición.
- [x] **9.9** Criterios 41–46 (contenido por sección).
  - *Verifica:* revisión visual de cada sección frente a spec §3.
- [x] **9.10** Anotar en este archivo cualquier criterio que falle, con la tarea que lo corrige.
  - *Verifica:* los 46 criterios están marcados como cumplidos.

### Registro de verificación (Fase 9)
Batería automática en Chrome sin interfaz que recorre los 46 criterios de spec §5 (49 comprobaciones, algunas cubren varios criterios), ejecutada a 1280 px y a 768 px. Resultado final: **49/49 en ambos anchos**.

| Criterio | Fallo encontrado | Causa | Corrección |
|---|---|---|---|
| 30 (traza oscura) | Al abrir por primera vez un detalle de error, el fondo de la traza era transparente durante el primer frame. | El CDN de Tailwind genera CSS a partir del DOM; las clases que solo aparecen en el contenido de los modales (`bg-terminal-bg`, botones del pie, textarea, desglose) no tenían regla hasta después de insertarse. | Añadidas al `safelist` de `tailwind.config` (mismo mecanismo que la Fase 6). Reverificado: 49/49. |
| 5 (responsive) | Detectado en la Fase 2: a 768 px la tabla de Contrataciones se salía 14 px de su contenedor. | 7 columnas en 544 px con `px-4`. | Padding de celdas `px-2 lg:px-4`, `main` `p-4 lg:p-8`, columna de acciones `w-10 lg:w-12` (tareas 2.9/1.10). |
| 39–40 (transición) | Detectado en la Fase 6: el primer despliegue no se animaba. | Mismo origen que el criterio 30 (`max-h-40` y `rotate-180` solo existían en JS). | `safelist` (tarea 6.3). |

Comprobaciones de repositorio para los criterios 1–2: el único `.html/.css/.js` fuera de `stitch_preference/` es `index.html`; la única URL cargada es `https://cdn.tailwindcss.com` (la de `competidor.example` es texto de una traza del dataset).
