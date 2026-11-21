# AgentHub — Panel de administración · Especificación

## 1. Descripción del producto
**AgentHub** es un SaaS B2B en el que las empresas alquilan agentes de IA: asistentes preconfigurados con unas instrucciones base (prompt de sistema) que se equipan con *skills*, capacidades como navegar por la web, leer documentos o gestionar calendarios. Las empresas despliegan estos agentes para tareas concretas de negocio: soporte, ventas, finanzas, marketing o asesoría legal.

El **administrador** es un empleado interno de AgentHub. Usa este panel para:
- vigilar las métricas de negocio y la salud de los agentes,
- gestionar los usuarios de las empresas cliente,
- configurar y eliminar agentes,
- mantener el catálogo de skills,
- revisar y finalizar contrataciones,
- revisar y resolver errores de ejecución.

## 2. Stack y restricciones

### 2.1 Stack
- Un único archivo `index.html` en la raíz del repositorio.
- HTML semántico, Tailwind CSS cargado desde su CDN oficial y JavaScript vanilla.
- Sin frameworks, sin jQuery, sin herramientas de build y sin librerías de iconos. Los iconos se dibujan como SVG dentro del propio HTML, con trazo fino de estilo lineal.

### 2.2 Restricciones globales
1. **Estilos:** solo con utilidades de Tailwind. No hay hojas de estilo propias, ni bloques de estilo, ni atributos de estilo en línea, tampoco añadidos por script. Mostrar, ocultar, girar o desplegar elementos se hace cambiando utilidades de Tailwind.
2. **Semántica:** el sidebar es un área lateral con navegación, la barra superior es una cabecera, el contenido es el área principal con una sección por cada apartado, los datos tabulares van en tablas con cabecera y cuerpo, las tarjetas son artículos y todo lo clicable son botones.
3. **Datos:** no hay backend ni llamadas de red. Todo el contenido sale del dataset de §2.4, guardado en memoria. Las secciones se leen de ese único dataset, así que empresas, agentes y skills se llaman igual en todo el panel.
4. **Dispositivos:** el panel es usable en escritorio (desde 1024 px) y en tablet (de 768 a 1023 px). A 768 px no aparece scroll horizontal de página.
5. **Idioma y formatos:**
   - Interfaz en español.
   - Importes en euros con coma decimal y símbolo detrás, por ejemplo "540,00 €".
   - Fechas en DD/MM/YY, por ejemplo "01/03/26".
   - Horas en HH:mm, en formato de 24 horas.
6. **Glosario:** la empresa cliente se etiqueta "Empresa" en Usuarios, "Cliente" en Contrataciones y "Propietario" en Gestión de agentes. Es la misma entidad y siempre lleva el mismo nombre exacto.
7. **Confirmaciones:** "Eliminar" (en cualquier sección), "Finalizar contrato" y "Marcar como resuelto" abren siempre un modal de confirmación. Solo al confirmar cambian los datos y la vista.
8. **Dropdowns:** se cierran al hacer clic fuera, al elegir una opción, al pulsar Escape o al abrir otro. Solo puede haber uno abierto.
9. **Modales:** se cierran con el botón de cierre (✕), con Cancelar si existe, con clic en el fondo oscurecido y con Escape. Un clic dentro del panel del modal no lo cierra.
10. **Modo oscuro:** el panel arranca en modo oscuro. Un toggle en la barra superior cambia toda la interfaz entre claro y oscuro mediante las variantes oscuras de Tailwind. El modo elegido se mantiene al navegar entre secciones. No hace falta recordarlo al recargar la página.
11. **Texto editado:** el prompt que edita el administrador se muestra siempre como texto literal, nunca se interpreta como HTML.

### 2.3 Lenguaje visual
- **Fondo general:** gris muy claro en modo claro y casi negro azulado en modo oscuro.
- **Superficies** (sidebar, tarjetas, tablas, modales y menús): blancas en claro y gris pizarra oscuro en oscuro, con borde sutil y esquinas redondeadas.
- **Texto:** el principal es casi negro en claro y casi blanco en oscuro. El secundario es gris medio en ambos modos.
- **Color de marca:** índigo. Se usa en el ítem activo del sidebar, en el botón primario, en el callout y en los focos.
- **Badges:** píldoras pequeñas con fondo suave del color y texto del mismo color más intenso. En modo oscuro llevan fondo translúcido oscuro y texto claro.
  - Verde: Activo.
  - Gris: Inactivo y Finalizado.
  - Rojo: Fallando y Grave.
  - Amarillo: Leve.
  - Naranja: Medio.
  - Azul: nº de agentes.
- **Botones:**
  - Primario: fondo índigo y texto blanco.
  - Secundario: borde gris y fondo neutro.
  - Peligroso: fondo rojo y texto blanco.
- **Texto en contraste:** todo el texto es legible en ambos modos.

### 2.4 Dataset (fuente única de verdad)
Fecha de referencia del dataset: **21/11/26**.

**Empresas**
| id | nombre |
|---|---|
| e1 | Nexora Logística |
| e2 | Bravia Retail |
| e3 | Lumen Salud |
| e4 | Tercia Finanzas |
| e5 | Ondara Legal |

**Skills** (precio en €/mes)
| id | nombre | precio | descripción breve | descripción amplia |
|---|---|---|---|---|
| s1 | Navegación web | 29 € | Consulta y extrae información de páginas web. | Permite al agente abrir direcciones web, seguir enlaces, usar buscadores y extraer contenido estructurado de páginas públicas. Útil para vigilar la competencia, verificar datos o investigar. |
| s2 | Lectura de documentos | 19 € | Lee PDF, Word y hojas de cálculo. | Extrae texto, tablas y metadatos de PDF (incluidos los escaneados, mediante reconocimiento de texto), documentos Word y hojas de cálculo para resumirlos, clasificarlos o responder preguntas sobre su contenido. |
| s3 | Gestión de calendarios | 15 € | Crea y reprograma eventos. | Se conecta a los calendarios corporativos para consultar disponibilidad, crear, mover o cancelar eventos y enviar invitaciones sin crear solapamientos. |
| s4 | Envío de correos | 12 € | Redacta y envía emails. | Redacta respuestas y campañas, las envía desde la cuenta autorizada de la empresa y respeta los límites de envío por hora. |
| s5 | Análisis de datos | 35 € | Calcula métricas y genera informes. | Procesa datos tabulares, calcula indicadores clave, detecta anomalías y genera informes resumidos listos para la dirección. |
| s6 | Integración CRM | 25 € | Sincroniza leads y oportunidades. | Lee y actualiza contactos, leads y oportunidades en el CRM del cliente para mantener el embudo comercial al día. |

**Agentes**
| id | nombre | propietario | estado | skills | prompt de sistema |
|---|---|---|---|---|---|
| a1 | Atlas Soporte | Nexora Logística | activo | Navegación web, Lectura de documentos, Envío de correos | Eres Atlas, agente de soporte de Nexora Logística. Resuelve incidencias de envíos con tono cordial y escala a un humano si no puedes resolverlas en dos intentos. |
| a2 | Clara Agenda | Lumen Salud | activo | Gestión de calendarios, Envío de correos | Eres Clara, asistente de citas de Lumen Salud. Gestiona las citas de los pacientes, confirma por email y no compartas nunca datos clínicos. |
| a3 | Orion Ventas | Bravia Retail | fallando | Navegación web, Integración CRM, Análisis de datos | Eres Orion, agente comercial de Bravia Retail. Cualifica leads, actualiza el CRM y resume las oportunidades cada mañana. |
| a4 | Vega Finanzas | Tercia Finanzas | fallando | Lectura de documentos, Análisis de datos | Eres Vega, analista financiero de Tercia Finanzas. Procesa facturas y genera el informe de tesorería semanal. |
| a5 | Lex Contratos | Ondara Legal | inactivo | Lectura de documentos | Eres Lex, asistente legal de Ondara Legal. Revisa contratos y señala cláusulas de riesgo sin dar asesoramiento jurídico vinculante. |
| a6 | Nova Marketing | Bravia Retail | activo | Navegación web, Análisis de datos, Envío de correos, Integración CRM | Eres Nova, agente de marketing de Bravia Retail. Analiza campañas, propone mejoras y envía el informe semanal al equipo. |

Totales:
- Por estado: 3 activos, 2 fallando y 1 inactivo.
- Agentes por skill:
  | Skill | Agentes |
  |---|---|
  | Navegación web | 3 |
  | Lectura de documentos | 3 |
  | Gestión de calendarios | 1 |
  | Envío de correos | 3 |
  | Análisis de datos | 3 |
  | Integración CRM | 2 |

**Usuarios**
| id | nombre | email | plan | estado | fecha de alta | empresa | agentes contratados |
|---|---|---|---|---|---|---|---|
| u1 | Laura Méndez | laura.mendez@nexora.example | Gold | Activo | 12/01/25 | Nexora Logística | Atlas Soporte |
| u2 | Diego Ramírez | diego.ramirez@bravia.example | Gold | Activo | 03/03/25 | Bravia Retail | Orion Ventas, Nova Marketing |
| u3 | Sofía Castro | sofia.castro@lumen.example | Silver | Activo | 20/04/25 | Lumen Salud | Clara Agenda |
| u4 | Javier Ortega | javier.ortega@tercia.example | Gold | Activo | 08/07/25 | Tercia Finanzas | Vega Finanzas |
| u5 | Marta Iglesias | marta.iglesias@ondara.example | Silver | Inactivo | 15/08/25 | Ondara Legal | Lex Contratos |
| u6 | Pablo Serrano | pablo.serrano@bravia.example | Silver | Inactivo | 02/10/25 | Bravia Retail | Nova Marketing |

**Contratos**

Regla: importe total pagado = suma de los precios mensuales de las skills contratadas × meses facturados. En los contratos activos, la fecha de fin está vacía.

| id | cliente | agente | skills contratadas | inicio | fin | estado | €/mes | meses | importe |
|---|---|---|---|---|---|---|---|---|---|
| c1 | Nexora Logística | Atlas Soporte | Navegación web, Lectura de documentos, Envío de correos | 01/03/26 | (vacía) | Activo | 60 € | 9 | 540,00 € |
| c2 | Lumen Salud | Clara Agenda | Gestión de calendarios, Envío de correos | 15/05/26 | (vacía) | Activo | 27 € | 7 | 189,00 € |
| c3 | Bravia Retail | Orion Ventas | Navegación web, Integración CRM, Análisis de datos | 01/02/26 | (vacía) | Activo | 89 € | 10 | 890,00 € |
| c4 | Tercia Finanzas | Vega Finanzas | Lectura de documentos, Análisis de datos | 10/01/26 | (vacía) | Activo | 54 € | 11 | 594,00 € |
| c5 | Ondara Legal | Lex Contratos | Lectura de documentos | 01/09/25 | 28/02/26 | Finalizado | 19 € | 6 | 114,00 € |
| c6 | Bravia Retail | Nova Marketing | Navegación web, Análisis de datos, Envío de correos, Integración CRM | 01/06/26 | (vacía) | Activo | 101 € | 6 | 606,00 € |
| c7 | Nexora Logística | Atlas Soporte | Navegación web, Lectura de documentos | 01/01/25 | 31/12/25 | Finalizado | 48 € | 12 | 576,00 € |

El importe se obtiene siempre de los precios del catálogo y de los meses. Nunca se escribe una cifra independiente.

**Log de errores** (se muestra ordenado de más reciente a más antiguo)
| id | fecha y hora | agente | gravedad | nombre del error | descripción |
|---|---|---|---|---|---|
| r1 | 21/11/26 09:42 | Orion Ventas | Grave | CRMAuthError | Token de acceso al CRM expirado; sincronización de leads detenida. |
| r2 | 21/11/26 08:15 | Vega Finanzas | Grave | DataPipelineTimeout | Tiempo de espera agotado al procesar el informe de tesorería semanal. |
| r3 | 20/11/26 17:30 | Orion Ventas | Medio | WebScrapeBlocked | Acceso denegado (403) al navegar por el catálogo de un competidor. |
| r4 | 20/11/26 11:05 | Atlas Soporte | Leve | MailRateLimit | Límite de envío alcanzado; correos en cola. |
| r5 | 19/11/26 16:48 | Vega Finanzas | Medio | PDFParseError | No se pudo extraer la tabla de una factura escaneada. |
| r6 | 18/11/26 10:20 | Nova Marketing | Leve | SlowResponse | Latencia superior a 8 segundos en una consulta de análisis. |
| r7 | 17/11/26 14:02 | Clara Agenda | Medio | CalendarSyncConflict | Conflicto al crear un evento duplicado. |

Trazas técnicas (se muestran literalmente, una línea por renglón):

- **r1:**
  ```
  CRMAuthError: 401 Unauthorized
    at CrmClient.refreshToken (crm/client.js:88)
    at SyncJob.run (jobs/sync-leads.js:41)
  ```
- **r2:**
  ```
  TimeoutError: pipeline exceeded 120000ms
    at Pipeline.execute (analytics/pipeline.py:212)
    at ReportTask.build (tasks/treasury.py:57)
  ```
- **r3:**
  ```
  HTTPError: 403 Forbidden https://competidor.example/catalogo
    at Browser.fetch (web/browser.js:134)
  ```
- **r4:**
  ```
  RateLimitError: 100 msgs/h exceeded
    at Mailer.send (mail/mailer.js:72)
  ```
- **r5:**
  ```
  ParseError: no table found on page 2
    at PdfReader.extractTables (docs/pdf.py:98)
  ```
- **r6:**
  ```
  Warning: response_time=8412ms > 8000ms
    at Monitor.check (core/monitor.js:30)
  ```
- **r7:**
  ```
  ConflictError: event overlaps id=ev_5521
    at Calendar.create (cal/api.js:61)
  ```

**Métricas fijas del Dashboard**
- **Ingresos totales (este mes): 331,00 €.** Es la suma de la cuota mensual de los contratos activos (60 + 27 + 89 + 54 + 101). El valor es fijo y no cambia al finalizar contratos.
- **Pérdidas por descuentos y cupones: 48,00 €.**

### 2.5 Reglas de coherencia tras cambios
1. Los contadores "Agentes activos" y "Agentes fallando" del Dashboard reflejan siempre los agentes visibles en Gestión de agentes con esos estados.
2. Los badges de nº de agentes de cada skill y la lista "Agentes que la usan" reflejan siempre los agentes visibles en Gestión de agentes.
3. **Eliminar un agente:**
   - Desaparece de Gestión de agentes.
   - Deja de contar en el Dashboard y en Skills.
   - Su nombre se sigue mostrando en Contrataciones, en el detalle de usuarios y en el Log de errores.
4. **Eliminar una skill:**
   - Desaparece del catálogo.
   - Su nombre y su precio se siguen mostrando en el desglose de los contratos que la incluyen.
   - Los agentes la siguen mostrando en su lista de skills.
5. **Eliminar un usuario:** solo afecta a la tabla de usuarios.
6. **Tablas vacías:** si una tabla se queda sin filas, muestra una única fila a todo el ancho con un mensaje de lista vacía:
   - Usuarios: "No hay usuarios registrados."
   - Agentes: "No hay agentes registrados."
   - Contrataciones: "No hay contratos registrados."
   - Log de errores: "No hay errores pendientes."
   - Si el catálogo de Skills queda vacío, muestra "No hay skills en el catálogo."

## 3. Especificaciones por sección

### 3.1 Dashboard (título de la barra superior: "Dashboard")
- **D1 · Rejilla de métricas.** Cuatro tarjetas de métrica en cuadrícula de 2×2 (en pantallas muy estrechas se apilan en una columna), con separación generosa. Orden:
  1. Ingresos totales (este mes): 331,00 €, acento azul, icono de billete.
  2. Pérdidas por descuentos y cupones: 48,00 €, acento morado, icono de cupón.
  3. Agentes activos: 3, acento verde, icono de círculo con check.
  4. Agentes fallando: 2, acento rojo, icono de triángulo de advertencia.
- **D2 · Aspecto de cada tarjeta.**
  - Fondo de superficie, sombra sutil y una franja vertical del color de acento en el borde izquierdo.
  - Arriba, el icono dentro de un cuadrado redondeado con fondo suave del color de acento.
  - Debajo, la etiqueta en texto secundario pequeño y el valor en cifra grande y negrita.
- **D3 · Valores coherentes.** "Agentes activos" y "Agentes fallando" se calculan a partir de los agentes de Gestión de agentes (regla 2.5.1). Coinciden siempre con los badges de esa tabla.
- **D4 · Placeholder del gráfico semanal.** Debajo de las tarjetas hay un bloque de ancho completo y unos 256 px de alto, con esquinas redondeadas y borde gris discontinuo. En el centro, en texto secundario, la etiqueta "Gráfico de actividad semanal".

### 3.2 Gestión de usuarios (título: "Gestión de usuarios")
- **U1 · Tabla de usuarios.** Muestra los 6 usuarios del dataset.
  - Columnas: Nombre, Email, Plan, Estado y una última columna de acciones sin título visible.
  - Plan: texto normal ("Silver" o "Gold"), sin badge.
  - Estado: badge verde "Activo" o gris "Inactivo".
  - Email: si no cabe, parte en varias líneas en vez de ensanchar la tabla.
  - Filas: separadas por líneas finas y con un ligero resaltado al pasar el ratón.
- **U2 · Dropdown de acciones.** Botón ⋮ en cada fila, con las opciones "Ver detalle" y "Eliminar". "Eliminar" aparece en rojo.
- **U3 · Modal de detalle de usuario.** Título: nombre del usuario. Cuerpo: lista de pares etiqueta/valor con:
  - Nombre.
  - Email.
  - Plan.
  - Estado (con su badge).
  - Fecha de alta (DD/MM/YY).
  - Empresa.
  - Agentes contratados, como lista con un nombre de agente por línea.
- **U4 · Eliminar usuario.** Abre el modal de confirmación con el mensaje "¿Eliminar a {nombre del usuario}? Esta acción no se puede deshacer." Si se confirma, la fila desaparece.

### 3.3 Gestión de agentes (título: "Gestión de agentes")
- **A1 · Tabla de agentes.** Muestra los 6 agentes del dataset.
  - Columnas: Nombre, Propietario, Estado, Skills y una columna de acciones sin título visible.
  - Propietario: el nombre de la empresa.
  - Estado: badge verde "Activo", gris "Inactivo" o rojo "Fallando".
- **A2 · Control de skills colapsable.** En la columna Skills hay un botón con el número de skills ("3 skills"; en singular, "1 skill") y una flecha.
  - **Colapsado** (estado por defecto): la flecha apunta hacia abajo.
  - **Desplegar:** al hacer clic se abre, justo debajo de la fila del agente, una sub-fila que ocupa todo el ancho de la tabla. Contiene los nombres de sus skills como chips en línea. La sub-fila crece con una transición suave de altura y opacidad (unos 300 ms) y la flecha gira hasta apuntar hacia arriba.
  - **Colapsar:** otro clic la cierra con la misma transición, sin dejar ningún hueco visible.
  - Cada agente se despliega de forma independiente.
- **A3 · Dropdown de acciones.** Botón ⋮ con las opciones "Configurar" y "Eliminar" (en rojo).
- **A4 · Modal Configurar.**
  - Título: "Configurar: {nombre del agente}".
  - Cuerpo: etiqueta "Prompt de sistema" y un área de texto editable de unas 8 líneas, a todo el ancho, con el prompt actual.
  - Pie: botones "Cancelar" (secundario) y "Guardar" (primario).
  - Guardar conserva el texto editado y cierra el modal. Al reabrirlo aparece el texto guardado.
  - Cancelar, ✕, clic en el fondo o Escape cierran el modal sin guardar.
- **A5 · Eliminar agente.** Abre el modal de confirmación con el mensaje "¿Eliminar el agente {nombre}? Esta acción no se puede deshacer." Si se confirma, desaparecen su fila y su sub-fila de skills, y se aplica la regla 2.5.3.

### 3.4 Skills (título: "Skills")
- **S1 · Callout explicativo.** Encima del catálogo hay una caja a todo el ancho con fondo índigo muy suave, borde índigo claro e icono de información a la izquierda. Texto: "Una skill es una capacidad que se añade a un agente para ampliar lo que puede hacer, como navegar por la web, leer documentos o gestionar calendarios. Cada skill se factura por separado en €/mes."
- **S2 · Catálogo en grid.** Las tarjetas se reparten en 3 columnas en escritorio y en 2 en tablet. Cada tarjeta muestra:
  - el nombre de la skill como título,
  - la descripción breve en texto secundario,
  - abajo, un badge azul con el número de agentes que la tienen ("3 agentes"; en singular, "1 agente"; con cero, "0 agentes").
- **S3 · Dropdown de la tarjeta.** Botón ⋮ en la esquina superior derecha de cada tarjeta, con las opciones "Ver detalle" y "Eliminar" (en rojo).
- **S4 · Modal de detalle de skill.**
  - Título: nombre de la skill.
  - Cuerpo, en este orden:
    1. La descripción amplia.
    2. El apartado "Agentes que la usan", con la lista de nombres. Si no la usa ninguno, muestra "Ningún agente la usa actualmente.".
    3. El apartado "Precio", con el importe mensual, por ejemplo "29,00 € / mes".
- **S5 · Eliminar skill.** Abre el modal de confirmación con el mensaje "¿Eliminar la skill {nombre} del catálogo?". Si se confirma, la tarjeta desaparece y el grid se recoloca.

### 3.5 Contrataciones de agentes (título: "Contrataciones de agentes")
- **C1 · Tabla de contratos.** Muestra los 7 contratos en el orden del dataset.
  - Columnas: Cliente, Agente, Skills (número de skills contratadas), Inicio / Fin, Estado, Importe y una columna de acciones sin título visible.
  - Inicio / Fin: la fecha de inicio y, debajo en texto más pequeño, la de fin. En los contratos activos el hueco de la fecha de fin queda vacío.
  - Estado: badge verde "Activo" o gris "Finalizado".
  - Importe: en euros, alineado a la derecha con cifras de ancho fijo.
- **C2 · Dropdown de acciones.** Botón ⋮ con las opciones "Ver detalle" y "Finalizar contrato". Si el contrato está finalizado, "Finalizar contrato" se ve atenuada, con cursor de no permitido, y no responde al clic.
- **C3 · Modal de detalle de contrato.**
  - Título: "{Agente} — {Cliente}", separados por una raya larga. Ejemplo: "Atlas Soporte — Nexora Logística".
  - Pares etiqueta/valor: Cliente, Agente, Fecha de inicio, Fecha de fin (vacía si está activo) y Estado (con badge).
  - Tabla de desglose con columnas Skill, Precio/mes, Meses y Subtotal, una fila por skill contratada. Ejemplo: "Navegación web | 29,00 € | 9 | 261,00 €".
  - Al pie del desglose, en negrita, "Total": la suma de los subtotales, idéntica al importe de la tabla.
- **C4 · Finalizar contrato.** Abre el modal de confirmación con el mensaje "¿Finalizar el contrato de {Agente} con {Cliente}?" y el botón "Finalizar". Al confirmar:
  - el modal se cierra,
  - el badge pasa a gris "Finalizado",
  - la fecha de fin pasa a ser la fecha actual del sistema en DD/MM/YY,
  - la opción "Finalizar contrato" queda deshabilitada.

  El importe no cambia.

### 3.6 Log de errores (título: "Log de errores")
- **E1 · Tabla de errores.** Muestra las 7 entradas del dataset, siempre ordenadas de la más reciente a la más antigua.
  - Columnas: Fecha y hora (DD/MM/YY HH:mm), Agente, Gravedad, Descripción y una columna de acciones sin título visible.
- **E2 · Badges de gravedad.** Amarillo "Leve", naranja "Medio" y rojo "Grave".
- **E3 · Dropdown de acciones.** Botón ⋮ con las opciones "Ver detalle" y "Marcar como resuelto".
- **E4 · Modal de detalle de error.**
  - Título: nombre del error (por ejemplo "CRMAuthError").
  - Cuerpo, en este orden:
    1. Agente.
    2. Fecha y hora.
    3. Gravedad (con badge).
    4. Descripción.
    5. "Traza técnica": un bloque de texto monoespaciado, pequeño, sobre fondo casi negro y con texto verde claro, que respeta los saltos de línea y se desplaza horizontalmente si una línea es muy larga. El bloque es oscuro también en modo claro.
- **E5 · Marcar como resuelto.** Abre el modal de confirmación con el mensaje "¿Marcar como resuelto el error {nombre del error} de {agente}?" y el botón "Marcar como resuelto". Si se confirma, la entrada desaparece y el resto mantiene el orden.

## 4. Inventario de componentes reutilizables

**Sidebar**
- *Contenido y aspecto:*
  - Panel fijo a la izquierda, a toda la altura de la ventana, de unos 224 px en tablet y 256 px en escritorio.
  - Arriba, el logo: un icono de agente con el texto "AgentHub" en negrita.
  - Debajo, 6 enlaces apilados con icono y nombre:
    | Enlace | Icono |
    |---|---|
    | Dashboard | cuadrícula |
    | Usuarios | personas |
    | Agentes | chip |
    | Skills | pieza de puzle |
    | Contrataciones | documento |
    | Log de errores | triángulo de advertencia |
  - El contenido principal empieza a la derecha del sidebar.
- *Comportamiento:*
  - Al hacer clic en un enlace se muestra su sección, se ocultan las otras cinco y se actualiza el título de la barra superior.
  - El enlace activo lleva fondo índigo suave, texto índigo, negrita y una barra índigo en su borde izquierdo. Para lectores de pantalla se marca como página actual.
  - Los demás enlaces se resaltan ligeramente al pasar el ratón.
  - Al cargar, la sección activa es Dashboard.

**Barra superior**
- *Contenido y aspecto:* franja fija en la parte superior del área de contenido, con el título de la sección actual a la izquierda y el toggle de modo claro/oscuro a la derecha. Títulos posibles: Dashboard, Gestión de usuarios, Gestión de agentes, Skills, Contrataciones de agentes y Log de errores.
- *Comportamiento:* el título cambia al navegar. La barra permanece visible al hacer scroll.

**Tarjeta de métrica**
- *Contenido y aspecto:* recibe un icono, una etiqueta, un valor y un color de acento (azul, morado, verde o rojo). Aspecto descrito en D2.
- *Comportamiento:* estática.

**Dropdown de acciones**
- *Contenido y aspecto:*
  - Botón con el icono ⋮, sin texto visible y con nombre accesible "Acciones de {elemento}".
  - El menú flotante es una superficie con sombra, alineada al borde derecho del botón y de unos 176 px de ancho.
  - Cada opción es un botón de texto que se resalta al pasar el ratón. Las acciones destructivas van en rojo y las deshabilitadas, atenuadas.
- *Comportamiento:*
  - Un clic en ⋮ abre el menú y cierra cualquier otro abierto.
  - Se cierra según la restricción 2.2.8.
  - Se abre hacia abajo, salvo cuando no hay espacio (por ejemplo en las dos últimas filas de una tabla), en cuyo caso se abre hacia arriba. Nunca queda recortado por el contenedor.
  - Elegir una opción cierra el menú y ejecuta la acción.

**Modal**
- *Contenido y aspecto:*
  - Fondo que cubre toda la ventana, negro semitransparente, con un panel centrado de hasta unos 512 px de ancho.
  - El panel tiene esquinas redondeadas y altura máxima del 90 % de la ventana; si el contenido no cabe, se desplaza por dentro.
  - Cabecera: título a la izquierda y botón ✕ a la derecha, con nombre accesible "Cerrar".
  - Cuerpo, y un pie opcional con botones alineados a la derecha.
  - Un único modal reutilizable cambia su contenido según la acción.
- *Comportamiento:*
  - Al abrirse, la página de fondo no hace scroll y el foco pasa al modal.
  - Se cierra según la restricción 2.2.9.
  - Al cerrarse, el foco vuelve al botón que lo abrió.

**Modal de confirmación**
- *Contenido y aspecto:* variante del Modal con:
  - un título:
    - "Confirmar eliminación" para Eliminar,
    - "Finalizar contrato" para Finalizar contrato,
    - "Marcar como resuelto" para Marcar como resuelto;
  - un mensaje de una frase;
  - un pie con "Cancelar" (secundario) y el botón de confirmación: "Eliminar" en rojo, y "Finalizar" o "Marcar como resuelto" en índigo.
- *Comportamiento:*
  - Confirmar aplica el cambio, cierra el modal y actualiza la vista.
  - Cancelar, ✕, clic en el fondo o Escape cierran sin cambios.

**Badge**
- *Contenido y aspecto:* píldora pequeña con texto corto y uno de los colores de §2.3 (verde, gris, rojo, amarillo, naranja o azul).
- *Comportamiento:* estático.

**Lista de skills colapsable**
- *Contenido y aspecto:* botón con el contador y una flecha, más una sub-fila a todo el ancho de la tabla con chips de skills.
- *Comportamiento:* descrito en A2. Para lectores de pantalla, el botón indica si la lista está desplegada.

**Callout**
- *Contenido y aspecto:* caja informativa con icono, fondo índigo suave, borde índigo claro y texto de 1 a 2 frases. Contenido descrito en S1.
- *Comportamiento:* estático.

**Toggle de modo oscuro**
- *Contenido y aspecto:* botón redondo en la barra superior, con nombre accesible "Cambiar modo claro/oscuro". En modo oscuro muestra un sol (la acción de pasar a claro) y en modo claro una luna.
- *Comportamiento:*
  - Un clic cambia toda la interfaz al modo contrario de forma inmediata e indica para lectores de pantalla si el modo oscuro está activo.
  - Arranca en modo oscuro.
  - El modo se mantiene al cambiar de sección.

## 5. Criterios de aceptación
1. El repositorio contiene un único `index.html` como implementación, sin hojas de estilo propias, sin bloques de estilo y sin atributos de estilo en línea, ni al cargar ni después de usar cualquier función del panel.
2. El único recurso externo cargado es Tailwind CSS desde su CDN. No se usan frameworks ni jQuery.
3. La página usa un área lateral con navegación, una cabecera, un área principal con 6 secciones y tablas con cabecera y cuerpo.
4. Todos los textos están en español. Los importes se muestran como "540,00 €", las fechas como DD/MM/YY y las horas como HH:mm.
5. A 768 px y a 1280 px de ancho no aparece scroll horizontal de página y ninguna columna queda cortada.
6. Al cargar, se ve el Dashboard, el sidebar lo marca como activo y la barra superior muestra el título "Dashboard".
7. Al hacer clic en cada uno de los 6 enlaces del sidebar se ve solo su sección, el título de la barra superior cambia al correspondiente y solo ese enlace aparece marcado como activo.
8. Al cargar la página, la interfaz está en modo oscuro.
9. Al pulsar el toggle, toda la interfaz (sidebar, barra superior, tarjetas, tablas, menús y modales) pasa a modo claro sin textos ilegibles. Al pulsarlo de nuevo vuelve a modo oscuro.
10. Tras cambiar a modo claro y recorrer las 6 secciones, el panel sigue en modo claro.
11. Los nombres de empresa son idénticos en Usuarios (campo Empresa del detalle), en Gestión de agentes (columna Propietario) y en Contrataciones (columna Cliente).
12. El Dashboard muestra 3 agentes activos y 2 fallando, igual que los badges de Gestión de agentes.
13. Tras eliminar un agente activo y volver al Dashboard, "Agentes activos" muestra 2.
14. Los badges de Skills muestran: Navegación web 3, Lectura de documentos 3, Gestión de calendarios 1, Envío de correos 3, Análisis de datos 3 e Integración CRM 2.
15. En cada contrato, el Total del modal de detalle coincide exactamente con el Importe de la tabla.
16. En el desglose de cada contrato, el precio/mes de cada skill coincide con el precio que muestra el modal de detalle de esa skill.
17. Todos los agentes que aparecen en el Log de errores existen en Gestión de agentes, y Orion Ventas y Vega Finanzas aparecen en el log.
18. Al pulsar ⋮ en cualquier fila o tarjeta se abre su menú con las opciones propias de su sección.
19. Con un menú abierto, un clic en cualquier punto fuera del menú lo cierra.
20. Al abrir un segundo menú ⋮, el primero se cierra. Escape cierra el menú abierto.
21. Al elegir una opción del menú, el menú se cierra y se ejecuta la acción.
22. Los menús de las dos últimas filas de cada tabla se ven completos, sin quedar recortados.
23. Cada "Ver detalle" (Usuarios, Skills, Contrataciones y Log de errores) y cada "Configurar" (Agentes) abre un modal con el contenido especificado en U3, S4, C3, E4 y A4.
24. Todo modal se cierra con el botón ✕.
25. Todo modal se cierra con un clic en el fondo oscurecido. Un clic dentro del panel no lo cierra.
26. Todo modal se cierra con la tecla Escape.
27. El modal "Configurar: {nombre}" muestra el prompt del agente en un área de texto editable.
28. Si se edita el prompt y se pulsa Guardar, el modal se cierra y, al reabrirlo, aparece el texto editado.
29. Si se edita el prompt y se pulsa Cancelar, al reabrir el modal aparece el texto anterior.
30. En el detalle de un error, la traza técnica se ve en un bloque monoespaciado de fondo oscuro, también en modo claro.
31. "Eliminar" en Usuarios, Gestión de agentes y Skills abre siempre el modal de confirmación.
32. Si en el modal de confirmación se pulsa Cancelar, no cambia nada.
33. Si en el modal de confirmación de "Eliminar" se pulsa el botón de confirmar, el elemento desaparece de su tabla o catálogo.
34. "Finalizar contrato" en un contrato activo abre el modal de confirmación.
35. Al confirmar "Finalizar contrato", el modal se cierra, el badge pasa a gris "Finalizado" y aparece como fecha de fin la fecha actual en DD/MM/YY.
36. En un contrato finalizado, la opción "Finalizar contrato" se ve deshabilitada y el clic no tiene efecto.
37. "Marcar como resuelto" abre el modal de confirmación. Al confirmar, la entrada desaparece del log y las restantes siguen ordenadas de más reciente a más antigua.
38. Al cargar Gestión de agentes, todas las listas de skills están colapsadas y cada control muestra el número de skills con una flecha hacia abajo.
39. Al hacer clic en el control de skills, se despliega con transición suave una sub-fila a todo el ancho de la tabla, justo debajo del agente, con los nombres de sus skills, y la flecha apunta hacia arriba.
40. Un segundo clic en el control colapsa la sub-fila con transición suave, sin dejar hueco visible, y la flecha vuelve a apuntar hacia abajo.
41. El Dashboard muestra 4 tarjetas en cuadrícula de 2×2, con acento azul (ingresos totales), morado (pérdidas por descuentos y cupones), verde (agentes activos) y rojo (agentes fallando), cada una con sombra sutil.
42. Debajo de las tarjetas del Dashboard hay un bloque de ancho completo con borde discontinuo y la etiqueta "Gráfico de actividad semanal" centrada.
43. La tabla de usuarios tiene al menos 5 filas, con el plan como texto normal y el estado como badge verde "Activo" o gris "Inactivo".
44. En Skills, el callout aparece encima del catálogo. El catálogo muestra 3 columnas a 1280 px y 2 columnas a 768 px, y cada tarjeta tiene su botón ⋮ en la esquina superior derecha.
45. La tabla de contrataciones tiene al menos 4 contratos, activos y finalizados, y los activos no muestran fecha de fin.
46. El Log de errores tiene al menos 6 entradas ordenadas de más reciente a más antigua, con badges amarillo "Leve", naranja "Medio" y rojo "Grave".
