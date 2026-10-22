
================================================================================
1. CONTEXTO Y ALCANCE
================================================================================

1.1. OBJETIVO

Turbine es una empresa de IA en fase inicial (7 personas) cuyo modelo de negocio es
una suscripción mensual cuyo importe depende del rango de facturación anual del
cliente. A la suscripción se suman servicios adicionales facturados puntualmente.
Actualmente la facturación y el cobro son manuales, lo que no es sostenible para
aspirar a miles de clientes y a un departamento administrativo dedicado.

El sistema es una aplicación web de uso interno, independiente de Brain, que
permite registrar clientes, generar facturas puntuales y recurrentes, y hacer
seguimiento automatizado de cobros, con integración con Stripe (simulado) y
distintos niveles de acceso por rol.

Criterios de éxito (Alejandro Ruiz):
- Rapidez y sencillez: mínimo número de clics.
- Movilidad: uso desde cualquier dispositivo.
- Fluidez de la información: ningún dato relevante deja de llegar a la persona
  correcta.
- Automatización confiable y explicable.

1.2. ALCANCE

DENTRO DEL ALCANCE:
- Gestión de clientes (empresas), contactos y suscripción.
- Facturas puntuales y recurrentes, con PDF y numeración.
- Envío automático de facturas y recordatorios (email/WhatsApp simulados).
- Detección de cobros vía Stripe (simulado), con posibilidad de otras fuentes.
- Seguimiento de impagos, incidencias, pausas y bajas.
- Gestión de usuarios internos, roles y asignación de clientes.
- Notificaciones internas, dashboard e informes.
- Configuración, auditoría y copias de seguridad.
- API documentada para sistemas externos.
- Acceso desde ordenador, tablet y móvil.
- Seguridad, integridad y conservación de la información.
- Despliegue on-premises y disponibilidad en horario laboral.

FUERA DEL ALCANCE:
- Integración con Brain.
- El sistema no cobra: solo consulta si una factura está pagada.
- Acceso de clientes de Turbine a la app (uso interno; ver D-01).
- Chat propio (comunicaciones por email/WhatsApp).
- Integración con Verifactu (lo lleva la gestoría).
- Fiscalidad internacional completa (solo datos no limitados a España).
- Usuario para la gestoría (se le envía paquete exportado).
- Pagos fraccionados.
- Ampliaciones opcionales aplazadas: presupuestos, varias sociedades emisoras,
  centros de coste, conversión de monedas, objetivos de facturación, cierre de
  periodos, fusión de duplicados, importación de facturas antiguas, rol externo
  para gestoría, IA, delegación temporal, adjuntos, etiquetas, favoritos, filtros
  guardados, acciones masivas, menciones, informes programados, alertas
  configurables, preferencias de canal, calendario laboral, reasignación de
  aprobaciones, notas restringidas, pruebas de plantillas y excepción de no
  prorrateo.

1.3. ACTORES Y PARTES INTERESADAS

| Actor / stakeholder | Tipo | Descripción | Intereses principales |
|---|---|---|---|
| Superadministrador (Superadmin) | Actor principal | Configura el sistema y consulta auditoría; máximos privilegios | Configurar plazos, tarifas, series, plantillas; auditoría |
| Administrador (Admin) | Actor principal | Gestiona clientes, facturas, usuarios, asignaciones y aprobaciones | Visión global de facturación y cobros |
| Usuario operativo | Actor principal | Empleado administrativo que solo gestiona los clientes asignados (antiguo "Lector") | Gestionar rápido sus clientes, facturas y cobros |
| Planificador del sistema | Actor secundario (sistema) | Lanza procesos automáticos (facturas del día 1, recordatorios, consulta de pagos, copias) | Procesos a tiempo, sin duplicados y con registro |
| Stripe (simulado) | Actor secundario (sistema externo) | Informa de pagos | Proporcionar información de cobros |
| Servicio de mensajería (simulado) | Actor secundario (sistema externo) | Entrega email/WhatsApp e informa de fallos | Entregar comunicaciones y avisar de rebotes |
| Sistema externo vía API | Actor secundario (sistema externo) | Consulta facturas pendientes y comunica bajas | Consultar sin saltarse reglas |
| Cliente de Turbine | Parte interesada (no usa la app) | Recibe facturas | Recibir facturas correctamente |
| Persona de contacto | Parte interesada (no usa la app) | Contacto de la empresa cliente | Recibir o proporcionar información |
| Gestoría | Parte interesada (no usa la app) | Asesoría externa contable/fiscal | Información contable completa |
| Dirección de Turbine | Parte interesada | Fundador | Reducir trabajo manual y crecer |

D-01 (decisión cerrada): se elimina el rol "Cliente" que aparecía en el documento
B. La app es solo de uso interno.


================================================================================
2. REQUISITOS DE NEGOCIO
================================================================================

| ID | Requisito |
|---|---|
| BR-01 | Reducir el trabajo manual y agilizar las tareas administrativas habituales, especialmente facturación y recordatorios |
| BR-02 | Automatizar el seguimiento de cobros y conocer la situación financiera (facturado, cobrado y pendiente) |
| BR-03 | Permitir el crecimiento de la administración con un sistema independiente de Brain y preparado para cambios futuros |
| BR-04 | Asegurar que la información importante llega a la persona correcta |
| BR-05 | Permitir gestionar la administración desde cualquier lugar y dispositivo |
| BR-06 | Cumplir las obligaciones de facturación y garantizar la trazabilidad e integridad de los documentos |
| BR-07 | Proteger los datos de los clientes |
| BR-08 | Gestionar las empresas cliente, sus datos de facturación y su relación comercial |
| BR-09 | Permitir revisar tarifas por facturación anual y gestionar excepciones comerciales |
| BR-10 | Proporcionar una interfaz sencilla e intuitiva centrada en clientes, facturas y pagos |

D-02 (decisión cerrada): la baja con facturas pendientes se permite; las deudas se
siguen reclamando (prevalece A sobre B).


================================================================================
3. REQUISITOS DE USUARIO
================================================================================

| ID | Actor | Requisito |
|---|---|---|
| UR-01 | Usuario interno | Acceder de forma segura con cuenta corporativa |
| UR-02 | Administrador | Gestionar usuarios, roles, permisos y asignación de clientes |
| UR-03 | Administrador, Usuario operativo | Gestionar clientes, contactos compartidos, datos fiscales y comerciales, y suscripciones |
| UR-04 | Administrador, Usuario operativo | Importar clientes desde CSV y conocer los errores de importación |
| UR-05 | Administrador, Usuario operativo | Crear facturas puntuales y recurrentes, revisarlas antes de emitir y corregir errores sin alterar las emitidas |
| UR-06 | Administrador, Usuario operativo | Enviar facturas y recordatorios automáticamente y consultar el resultado de los envíos y sus destinatarios |
| UR-07 | Administrador, Usuario operativo | Consultar el estado de cobro y gestionar disputas, incidencias e importes distintos |
| UR-08 | Administrador, Usuario operativo | Gestionar pausas, bajas y reactivaciones conservando datos y deudas |
| UR-09 | Usuario interno | Recibir avisos, consultar tareas pendientes y encontrar rápidamente clientes o facturas |
| UR-10 | Administrador | Consultar y exportar la situación financiera mediante dashboard e informes |
| UR-11 | Administrador, Superadmin | Aprobar operaciones económicas sensibles y revisar anualmente las tarifas |
| UR-12 | Superadmin | Configurar parámetros, consultar auditoría y comprobar procesos automáticos |
| UR-13 | Sistema externo vía API | Consultar facturas pendientes y comunicar bajas |


================================================================================
4. REQUISITOS DEL SISTEMA
================================================================================

4.1. REQUISITOS FUNCIONALES

--- Usuarios y acceso ---

| ID | Requisito |
|---|---|
| FR-01 | Gestionar el acceso seguro mediante email y contraseña corporativos, recuperación de contraseña con enlace caducable y cierre automático de sesión por inactividad |
| FR-02 | Permitir al administrador crear, modificar y desactivar usuarios (sin autorregistro); al desactivar, cerrar sesiones y conservar registro de acciones |
| FR-03 | Asignar a cada usuario uno de los tres roles: Superadmin, Admin o Usuario operativo |
| FR-04 | Permitir al administrador asignar y desasignar clientes a cada usuario operativo |
| FR-05 | Impedir que un usuario operativo acceda a clientes o facturas no asignados, incluso escribiendo la URL directamente |
| FR-06 | Impedir el acceso a funcionalidades para las que el usuario no tenga permisos |

--- Clientes y contactos ---

| ID | Requisito |
|---|---|
| FR-07 | Crear, consultar, modificar y dar de baja lógica a empresas cliente; guardar sus datos comerciales y fiscales, avisar de ID fiscal duplicado y conservar su histórico |
| FR-08 | Gestionar contactos y sus funciones, permitir contactos compartidos entre empresas, avisar si se intenta eliminar el único contacto de facturación y conservar los destinatarios de cada envío |
| FR-09 | Mostrar el historial del cliente con altas, facturas, pagos, incidencias, cambios de tarifa y bajas |
| FR-10 | Almacenar datos comerciales (fecha inicio, plan, tarifa, responsable comercial, condiciones de pago) separados de los fiscales |
| FR-11 | Mostrar el estado de las empresas cliente en un listado sin abrir cada ficha |

--- Suscripción ---

| ID | Requisito |
|---|---|
| FR-12 | Registrar la suscripción con fecha de inicio, tarifa mensual y condiciones de pago |
| FR-13 | Mostrar el importe prorrateado del primer mes antes de confirmar el alta |
| FR-14 | Permitir indicar fecha de inicio futura, sin generar facturas hasta esa fecha |
| FR-15 | Permitir condiciones de pago particulares por cliente (p. ej., a 30 días) por encima de las generales |
| FR-16 | Impedir activar suscripción si faltan datos fiscales obligatorios, indicando cuáles |

--- Facturas ---

| ID | Requisito |
|---|---|
| FR-17 | Crear facturas puntuales con una o varias líneas (concepto, cantidad, precio, descuento, impuesto) |
| FR-18 | Generar automáticamente (día 1 del mes) las facturas de suscripciones activas |
| FR-19 | Generar, en la fecha de inicio, factura proporcional por los días restantes del mes |
| FR-20 | No generar facturas a clientes pausados, pendientes de activación o dados de baja |
| FR-21 | Si falla la generación de una factura, continuar con el resto y avisar al responsable |
| FR-22 | Mostrar vista previa de la factura antes de emitir |
| FR-23 | Impedir emitir factura si falta dato fiscal obligatorio, indicando cuál |
| FR-24 | Permitir modificar o eliminar solo borradores; al emitir, asignar numeración única y correlativa y generar un PDF completo e inmutable, independiente de cambios posteriores en plantilla o logo |
| FR-25 | Permitir crear factura rectificativa a partir de una emitida, manteniendo vínculo |
| FR-26 | Filtrar facturas por estado, tipo (puntual/recurrente), cliente y fecha |
| FR-27 | Permitir seleccionar impuestos entre opciones preconfiguradas |
| FR-28 | Gestionar estados del ciclo de vida: borrador, pendiente de aprobación, emitida, enviada, vencida, pagada, envío fallido, disputa, pagada parcialmente, rectificada, anulada, incobrable |
| FR-29 | Impedir modificación silenciosa de facturas emitidas y mantener trazabilidad de cambios |

--- Envío y recordatorios ---

| ID | Requisito |
|---|---|
| FR-30 | Enviar automáticamente las facturas con PDF por los canales autorizados, incluir los datos de soporte y registrar destinatario, canal, fecha y resultado; ante fallos, avisar y permitir corregir y reenviar sin duplicar la factura |
| FR-31 | Enviar recordatorios automáticos: antes del vencimiento (día 3), el día del vencimiento y periódicos (cada X días) hasta el pago, calculando fechas según condiciones del cliente |
| FR-32 | Dejar de enviar recordatorios cuando la factura quede pagada |
| FR-33 | Usar plantillas editables en español e inglés con datos del cliente, número, importe y vencimiento |

--- Cobros ---

| ID | Requisito |
|---|---|
| FR-34 | Consultar pagos de Stripe, asociarlos automáticamente solo cuando la referencia e importe coincidan, ignorar avisos repetidos y dejar para revisión manual los pagos ambiguos o con diferencias de importe |
| FR-35 | Permitir asociar manualmente un pago a una factura y deshacer asociación incorrecta, registrando quién y por qué |
| FR-36 | Calcular saldo de cada cliente: total facturado, cobrado y pendiente (teniendo en cuenta rectificativas) |
| FR-37 | Permitir pausar recordatorios de factura en disputa, indicando motivo y fecha de revisión, avisando al responsable |
| FR-38 | Permitir abrir incidencias internas sobre cliente o factura (responsable, estado, comentarios, resolución), pudiendo pausar recordatorios |
| FR-39 | Permitir a un administrador declarar factura incobrable, con motivo y aprobación, sin marcarla como pagada |

--- Bajas, pausas y revisión anual ---

| ID | Requisito |
|---|---|
| FR-40 | Permitir pausar suscripción con motivo, fecha de inicio y fecha prevista de reactivación, avisando al responsable |
| FR-41 | Permitir registrar solicitud de baja de cliente, manualmente o vía API |
| FR-42 | Al hacerse efectiva la baja, dejar de generar facturas recurrentes |
| FR-43 | Si el cliente dado de baja tiene facturas pendientes, mantener recordatorios hasta pago |
| FR-44 | Permitir reactivar cliente dado de baja usando ficha anterior, sin duplicado |
| FR-45 | Mostrar anualmente lista de clientes cuyo rango de tarifa debe revisarse, permitir introducir facturación anual indicando fuente y fecha, y proponer rango |
| FR-46 | El cambio de tarifa debe aprobarse antes de aplicarse, aplicarse desde fecha y no modificar facturas anteriores |
| FR-47 | Registrar quién aprobó un cambio de tarifa, cuándo y precios anterior y nuevo |
| FR-48 | Permitir al Superadmin configurar y modificar los tramos de facturación anual y sus tarifas sin alterar importes de facturas históricas |
| FR-49 | Permitir registrar excepciones comerciales (descuentos temporales, meses gratuitos, precios pactados) |

--- Aprobaciones, notificaciones y pantalla de trabajo ---

| ID | Requisito |
|---|---|
| FR-50 | Exigir aprobación de administrador para operaciones económicas sensibles (descuentos, cambios manuales de importe, facturas puntuales sobre umbral…), sin exigirla para recurrentes de suscripción ya aprobada |
| FR-51 | Ofrecer bandeja de aprobaciones con estado (pendiente/aprobada/rechazada) y comentario del aprobador |
| FR-52 | Impedir que un usuario apruebe su propia solicitud cuando el importe supere el umbral |
| FR-53 | Enviar notificaciones internas al responsable ante pagos, vencimientos, envíos fallidos, disputas, revisiones anuales y aprobaciones resueltas; permitir marcar leídas/resueltas |
| FR-54 | Mostrar pantalla de trabajo diario con facturas próximas a vencer, vencidas, pagos pendientes de asociar, envíos fallidos y aprobaciones pendientes |
| FR-55 | Configurar umbrales y condiciones que determinan si un cambio económico requiere aprobación de Admin o Superadmin |

--- Dashboard, informes, búsqueda e importación ---

| ID | Requisito |
|---|---|
| FR-56 | Mostrar solo a administradores dashboard con facturado, cobrado, pendiente y vencido por semana, mes, trimestre y año; tiempo medio de cobro; clientes con mayor deuda; previsión de ingresos recurrentes del mes siguiente |
| FR-57 | Generar informes filtrados por periodo, cliente, responsable, país, sector, estado y tipo de factura |
| FR-58 | Exportar informes a CSV y PDF, incluyendo solo datos que el usuario pueda ver |
| FR-59 | Generar paquete mensual o trimestral para la gestoría con facturas emitidas, rectificativas, cobros y PDF |
| FR-60 | Búsqueda global por nombre, razón social, ID fiscal, contacto, email o número de factura |
| FR-61 | Filtrar clientes por ubicación, sector y estado |
| FR-62 | Importar clientes desde CSV con vista previa, señalando filas con errores o ID fiscal duplicado, e importando solo filas válidas |
| FR-63 | Guardar resumen de cada importación con filas creadas, rechazadas y motivo |

--- Configuración, auditoría y API ---

| ID | Requisito |
|---|---|
| FR-64 | Permitir al Superadmin configurar plazo de pago por defecto, cadencia de recordatorios, dominio de correo permitido, datos fiscales de Turbine, logo, series, impuestos, rangos de tarifa y umbrales de aprobación. Cambios aplican solo a futuro |
| FR-65 | Registrar y permitir consultar la auditoría de acciones importantes y de procesos automáticos, incluyendo elementos procesados, fallos, motivos y estado de las últimas ejecuciones y copias |
| FR-66 | Ofrecer API para consultar si un cliente tiene facturas pendientes |
| FR-67 | Permitir que sistema externo comunique por API solicitud de baja, que el sistema registrará para gestionarla con sus reglas |
| FR-68 | La API debe exigir autenticación y aplicar los mismos permisos y reglas que la aplicación |

4.2. REQUISITOS NO FUNCIONALES

| ID | Categoría | Requisito | Criterio verificable |
|---|---|---|---|
| NFR-01 | Usabilidad | Operaciones habituales rápidas, intuitivas y con mínimo de clics, sin formación previa | Un usuario administrativo da de alta un cliente con suscripción o una factura puntual en menos de 1 minuto y sin ayuda |
| NFR-02 | Usabilidad | Mensajes de error comprensibles e interfaz coherente, sin ventanas modales intrusivas para acciones rutinarias | Los errores indican campo y motivo sin códigos técnicos; los botones mantienen posiciones coherentes y los avisos son no bloqueantes |
| NFR-03 | Portabilidad | App web responsive usable en ordenador, tablet y móvil | Mínimo 360 px sin scroll horizontal |
| NFR-04 | Compatibilidad | Funciona en navegadores actuales (incl. Android/iOS); CSV conserva tildes; PDF se abre con cualquier lector | Verificado en versiones actuales |
| NFR-05 | Accesibilidad | Campos con etiqueta, contraste suficiente, uso con teclado | Todos los campos etiquetados y navegables |
| NFR-06 | Rendimiento | Búsquedas y apertura de fichas < 2 s con miles de clientes | Medido en conexión normal |
| NFR-07 | Rendimiento | Tareas pesadas en segundo plano sin bloquear la app | Generación masiva e informes en background |
| NFR-08 | Escalabilidad | Soporta miles de clientes, cientos de miles de facturas y decenas de usuarios | Listados paginados |
| NFR-09 | Fiabilidad | Operaciones y procesos automáticos no duplican resultados | Idempotencia en generación, envío y pagos |
| NFR-10 | Fiabilidad | Fallo de un elemento no detiene el proceso completo | Error registrado y resto continúa |
| NFR-11 | Consistencia | Edición concurrente: el segundo usuario recibe aviso | No sobrescribe cambios |
| NFR-12 | Exactitud | Importes con 2 decimales exactos; PDF, BD y dashboard coinciden | Cifras idénticas en todas las vistas |
| NFR-13 | Seguridad | Solo usuarios autenticados y con permiso acceden a recursos | Rechazo también por URL directa o API |
| NFR-14 | Seguridad | Contraseñas hasheadas, HTTPS, sin claves en repositorio, facturas con sesión/permiso o enlaces caducables | Verificado en auditoría de seguridad |
| NFR-15 | Seguridad | Bloqueo temporal tras varios intentos fallidos seguidos | Cuenta bloqueada temporalmente |
| NFR-16 | Privacidad | Solo datos necesarios para facturar; sin marketing; anonimización sin invalidar facturas | Datos mínimos y anonimizables |
| NFR-17 | Trazabilidad | De cada factura pagada se consulta el pago que lo justifica; de cada envío fallido, el motivo | Consultable en ficha |
| NFR-18 | Auditoría | Registro de auditoría no editable ni borrable | Ningún usuario puede modificarlo |
| NFR-19 | Recuperabilidad | Copia de seguridad automática diaria y restauración probada | Restauración verificada al menos una vez |
| NFR-20 | Internacionalización | Comunicaciones en español e inglés; fechas con zona horaria; datos fiscales no asumen formato español | Plantillas y campos adaptados |
| NFR-21 | Interoperabilidad | Sustituir Stripe simulado por real o añadir otra fuente sin modificar el resto | Cambio aislado |
| NFR-22 | Interoperabilidad | API documentada con operaciones, entradas y respuestas | Documentación publicada |
| NFR-23 | Mantenibilidad | Pruebas automáticas de prorrateo, generación mensual, vencimientos, impuestos, permisos y conciliación | Suite de pruebas existente |
| NFR-24 | Despliegue | Software libre, configuración separada del código y guía de puesta en marcha reproducible | Guía publicada |
| NFR-25 | Disponibilidad | Disponible en horario de funcionamiento establecido, salvo mantenimientos programados | Horario definido por la empresa |
| NFR-26 | Despliegue | Instalación y ejecución on-premises en servidor local | Instalable en servidor local |


================================================================================
5. REGLAS DE NEGOCIO
================================================================================

| ID | Regla |
|---|---|
| RB-01 | Un cliente es siempre una empresa y debe tener al menos un contacto de facturación activo para emitirle facturas |
| RB-02 | La tarifa mensual depende del rango de facturación anual y se mantiene igual todo el año; rangos y precios configurables |
| RB-03 | El rango de tarifa se revisa anualmente; el nuevo precio aplica desde su fecha de efecto y no modifica facturas anteriores |
| RB-04 | Las facturas recurrentes son mensuales y se emiten el día 1 |
| RB-05 | Alta a mitad de mes: factura proporcional = tarifa mensual × días restantes (incluido el día de alta) / días del mes. Alta el último día: se factura solo ese día |
| RB-06 | Plazo de pago por defecto: 5 días naturales desde la emisión. Condiciones pactadas del cliente prevalecen |
| RB-07 | Recordatorios por defecto: día de emisión, día 3, día del vencimiento y cada 3 días hasta el pago. Configurables |
| RB-08 | Un impago nunca cancela el servicio ni sanciona; se siguen enviando recordatorios hasta pago o intervención humana |
| RB-09 | Estados de factura: borrador, pendiente de aprobación, emitida, enviada, vencida, pagada, envío fallido, disputa, pagada parcialmente, rectificada, anulada e incobrable |
| RB-10 | Solo se modifica o elimina una factura en borrador. Una vez emitida se corrige con factura rectificativa |
| RB-11 | Número único y correlativo por serie, asignado al emitir. Reenviar no cambia el número |
| RB-12 | Factura pagada solo con cobro del importe total. Diferencias quedan pendientes de revisión. Incobrable, anulada o rectificada nunca se marca pagada |
| RB-13 | Necesitan aprobación: descuentos, cambios manuales de importe, facturas puntuales sobre umbral, rectificativas, declarar incobrable y cambios de tarifa. Recurrentes normales no |
| RB-14 | Baja: se dejan de generar recurrentes desde la fecha de baja, pero las pendientes se siguen reclamando |
| RB-15 | Pausa: no genera facturas nuevas, pero las anteriores siguen su proceso de cobro |
| RB-16 | Importes con 2 decimales; cada línea redondeada al céntimo (hacia arriba en empate); total = suma de líneas redondeadas |
| RB-17 | Cambios de configuración (plazos, tarifas, plantillas, logo) solo afectan a facturas nuevas |
| RB-18 | Superadmin hace todo lo del Admin, además configura y consulta auditoría. Admin gestiona todo. Usuario operativo solo ve/modifica sus clientes asignados; no se asigna clientes, no cambia tarifas, no crea series ni ve dashboard |
| RB-19 | Datos de clientes no se borran: se conservan según ley. Contactos pueden anonimizarse; facturas emitidas se conservan siempre |
| RB-20 | Estados del cliente: pendiente de activación, activo, pausado, baja solicitada o inactivo. Solo activos generan recurrentes |
| RB-21 | Informes y exportaciones se conservan tiempo configurable; facturas emitidas según obligación legal |


================================================================================
6. SUPOSICIONES Y DECISIONES DE ANÁLISIS
================================================================================

| ID | Suposición o decisión |
|---|---|
| A-01 | Tres roles: Superadmin, Admin, Usuario operativo. Se elimina el rol Cliente (D-01). "Lector" pasa a llamarse "Usuario operativo" |
| A-02 | "La app no puede crear clientes" se interpreta como que un cliente no puede autorregistrarse |
| A-03 | Solo se editan borradores; las emitidas se corrigen con rectificativa |
| A-04 | Los estados de factura son propuesta del equipo (RB-09) |
| A-05 | Prorrateo con días reales del mes, incluyendo día de alta |
| A-06 | Recordatorios cada 3 días tras vencimiento (configurable) |
| A-07 | Dashboard e informes por semana, mes, trimestre y año |
| A-08 | Stripe, email y WhatsApp simulados; email y WhatsApp unificados como "Servicio de mensajería" |
| A-09 | Facturación anual para revisión de tarifa introducida manualmente con fuente y fecha |
| A-10 | Importe modificable en facturas puntuales; en recurrentes viene de la suscripción y solo cambia por revisión anual o excepción aprobada |
| A-11 | Conciliación automática solo en casos claros (misma referencia e importe); resto a revisión manual |
| A-12 | Umbrales iniciales propuestos: 2.000 € (aprobación de puntuales) y 10.000 € (separación de funciones), configurables |
| A-13 | Búsqueda/apertura de ficha < 2 s |
| A-14 | Copia de seguridad automática diaria |
| A-15 | Copias no se modifican para borrar clientes; al restaurar se reaplican anonimizaciones |
| A-16 | Redondeo por línea al céntimo, hacia arriba en empate |
| A-17 | Paquete para gestoría: ZIP con CSV y PDF |
| A-18 | Actor "Planificador del sistema" para tareas automáticas |
| A-19 | Importación CSV: solo filas válidas; las erróneas se rechazan y aparecen en resumen |
| A-20 | Fuera de versión: ampliaciones opcionales (presupuestos, varias sociedades, centros de coste, monedas, objetivos, cierre de periodos, fusión de duplicados, importación de facturas antiguas, rol gestoría, IA) |
| A-21 | Aplazadas: delegación temporal, adjuntos, etiquetas, favoritos, filtros guardados, acciones masivas, menciones, informes programados, alertas configurables, preferencias de canal, calendario laboral, reasignación de aprobaciones, notas restringidas, pruebas de plantillas y excepción de no prorrateo |
| A-22 | Plazo de pago: 5 días naturales |
| A-23 | Se incorporan on-premises y disponibilidad horaria de B |
| A-24 | Se incorporan separación datos comerciales/fiscales y contacto compartido entre empresas de B |
| A-25 | Documentación comercial adjunta y excepción de no prorrateo quedan documentadas pero aplazadas |
| A-26 | Se elimina el rol Cliente de B (D-01) |
| A-27 | Se deroga la prohibición de baja con deuda de B (D-02) |


================================================================================
7. DUDAS Y CUESTIONES PENDIENTES
================================================================================

| ID | Cuestión | Estado / resolución |
|---|---|---|
| Q-01 | Baja con facturas pendientes | Resuelta: se permite y se sigue reclamando (RB-14) |
| Q-02 | Baja gestionada por esta app o sistema externo | Resuelta: la registra y gestiona su efecto (FR-63, FR-92) |
| Q-03 | Roles y permisos del "lector" | Resuelta (A-01) |
| Q-04 | Edición de facturas hasta cambio de trimestre | Resuelta (A-03) |
| Q-05 | "La app no puede crear clientes" | Resuelta (A-02) |
| Q-06 | Periodos de informes | Resuelta (A-07) |
| Q-07 | Modificación de importe de factura | Resuelta (A-10) |
| Q-08 | Cadencia de recordatorios | Resuelta (A-06) |
| Q-09 | Factura puntual pendiente impide baja | Resuelta: no impide; se reclama |
| Q-10 | Tabla de tarifas actual | Pendiente de solicitar a Turbine |
| Q-11 | Tiempo de conservación de datos | Pendiente: confirmar con gestoría; parámetro configurable |
| Q-12 | Campos exactos del PDF | Pendiente de factura de ejemplo |
| Q-13 | Datos fiscales obligatorios | Pendiente: inicialmente razón social, ID fiscal y dirección fiscal |
| Q-14 | Importes concretos de aprobación y separación de funciones | Pendiente: propuesta A-12 |
| Q-15 | Reactivación de pausa automática o confirmada | Resuelta: se avisa al responsable (FR-62) |
| Q-16 | Consentimiento WhatsApp | Pendiente: marcado manual en ficha de contacto |
| Q-17 | Medio de notificaciones/recordatorios | Resuelta: email y WhatsApp simulados (A-08) |
| Q-18 | 5 días naturales o laborables | Resuelta: naturales (A-22) |
| Q-19 | "Saldo adeudado" para impedir baja | Resuelta (D-02) |
| Q-20 | Contenido del cuadro de mando | Resuelta (FR-79, FR-80) |
| Q-21 | Contenido de informes mensuales/anuales | Parcialmente resuelta (FR-81, FR-82); detalle pendiente |
| Q-22 | Operaciones exclusivas del Superadmin | Resuelta (RB-18) |
| Q-23 | Operaciones no críticas del Usuario operativo | Resuelta (RB-18) |
| Q-24 | Campos y función de la persona de contacto | Parcialmente resuelta (FR-13); detalle pendiente |
| Q-25 | Estados y transiciones de factura | Resuelta (RB-09, FR-42) |
| Q-26 | Condiciones de aprobación | Parcialmente resuelta (RB-13, FR-73, FR-78); umbrales pendientes (Q-14) |
| Q-27 | Restricciones de eliminación de borradores y baja lógica | Resuelta (FR-34, FR-44, FR-09, RB-19) |


================================================================================
8. TRAZABILIDAD
================================================================================

8.1. NEGOCIO → USUARIO → FUNCIONAL/NO FUNCIONAL

| Requisito de negocio | Requisito de usuario | Requisito funcional / no funcional |
|---|---|---|
| BR-01 | UR-05, UR-06, UR-07 | FR-21 a FR-51, FR-53 a FR-61 |
| BR-02 | UR-08, UR-09, UR-10 | FR-52, FR-53, FR-55 a FR-78, NFR-11, NFR-22 |
| BR-03 | UR-02, UR-04, UR-17 | FR-04 a FR-19, FR-86 a FR-90, NFR-10, NFR-13 |
| BR-04 | UR-11, UR-14 | FR-31, FR-48, FR-76, FR-77, FR-78 |
| BR-05 | UR-03, UR-12 | FR-09 a FR-19, FR-84, NFR-01 a NFR-04, NFR-08 |
| BR-06 | UR-01 | FR-01 a FR-03, NFR-05 a NFR-07 |
| BR-07 | UR-13, UR-15 | FR-79 a FR-83, NFR-15, NFR-28 |
| BR-08 | UR-06, UR-17 | FR-35 a FR-44, FR-89, NFR-14, NFR-21 |
| BR-09 | UR-01, UR-02 | FR-10, FR-14, NFR-16 a NFR-21 |
| BR-10 | UR-16, UR-18 | FR-88, FR-91 a FR-93, NFR-24 a NFR-30 |
| BR-11 | UR-03, UR-04 | FR-09 a FR-19 |
| BR-12 | UR-21 | FR-18 |
| BR-13 | UR-15, UR-16 | FR-67 a FR-71 |
| BR-14 | UR-14 | FR-72, FR-73, FR-78 |
| BR-15 | UR-17 | FR-35, FR-42 a FR-44, NFR-11 |
| BR-16 | UR-03 a UR-09 | NFR-01 a NFR-04, NFR-08, NFR-09 |

8.2. USUARIO → FUNCIONAL

| Requisito de usuario | Requisitos funcionales |
|---|---|
| UR-01 | FR-01, FR-02, FR-03 |
| UR-02 | FR-04, FR-05, FR-06, FR-07, FR-08 |
| UR-03 | FR-09 a FR-15 |
| UR-04 | FR-86, FR-87 |
| UR-05 | FR-21 a FR-29 |
| UR-06 | FR-32 a FR-38 |
| UR-07 | FR-45 a FR-51 |
| UR-08 | FR-39, FR-50, FR-52, FR-53, FR-58 |
| UR-09 | FR-55 a FR-61 |
| UR-10 | FR-12, FR-30, FR-62 a FR-66 |
| UR-11 | FR-31, FR-48, FR-59, FR-62, FR-76, FR-77 |
| UR-12 | FR-39, FR-84, FR-85 |
| UR-13 | FR-79 a FR-83 |
| UR-14 | FR-61, FR-68, FR-73, FR-74, FR-75 |
| UR-15 | FR-67, FR-68 |
| UR-16 | FR-51, FR-88 |
| UR-17 | FR-04, FR-89, FR-90 |
| UR-18 | FR-63, FR-91, FR-92, FR-93 |
| UR-19 | FR-08 |
| UR-21 | FR-18, FR-41 |
| UR-22 | FR-16 |
| UR-23 | FR-17 |

8.3. EJEMPLO COMPLETO: BAJA DE CLIENTE CON DEUDA

| Fuente | Requisito de usuario | Requisitos funcionales | Regla de negocio | Decisión |
|---|---|---|---|---|
| [P098, P176–P179, A20, A75] | UR-10, UR-18 | FR-63, FR-64, FR-65, FR-92 | RB-14 | Q-01, Q-02, D-02 |


================================================================================
9. GLOSARIO
================================================================================

| Término | Definición |
|---|---|
| Turbine | Empresa que encarga y usa el sistema internamente |
| Cliente / Empresa cliente | Empresa a la que Turbine factura. No usa la app |
| Contacto / Persona de contacto | Persona asociada a un cliente con una función (facturación, impagos…). Puede estar en varias empresas |
| Usuario interno | Empleado de Turbine: Superadmin, Admin o Usuario operativo |
| Responsable interno | Usuario asignado a un cliente que gestiona sus facturas y recibe avisos |
| Suscripción | Relación comercial mensual sin permanencia que origina las recurrentes |
| Relación / Ficha comercial | Condiciones acordadas: servicio, tarifa, condiciones de pago, responsable y documentación |
| Datos comerciales | Servicio, tarifa, responsable comercial |
| Datos fiscales | Razón social, ID fiscal, dirección fiscal |
| Rango / Tramo de facturación | Intervalo de facturación anual que determina la tarifa |
| Factura recurrente | Factura mensual generada el día 1 desde una suscripción |
| Factura puntual | Factura por servicio adicional creada manualmente |
| Factura proporcional / Prorrateo | Factura por días restantes del mes al alta |
| Borrador | Factura no emitida, modificable y eliminable |
| Emisión | Momento en que la factura recibe número definitivo y deja de modificarse |
| Serie | Secuencia de numeración de facturas |
| Factura rectificativa | Factura que corrige o anula total/parcialmente una emitida, vinculada a ella |
| Vencimiento | Fecha límite de pago (5 días naturales por defecto) |
| Recordatorio | Mensaje sobre factura pendiente |
| Impago | Factura vencida sin pago |
| Factura pendiente | Factura sin pago confirmado |
| Disputa | Cliente cuestiona factura; recordatorios pausados hasta revisión |
| Incobrable | Factura dada por imposible de cobrar con aprobación; no es "pagada" |
| Conciliación | Asociar pago recibido con su factura |
| Saldo | Diferencia entre facturado y pagado |
| Pausa | Suspensión temporal sin baja |
| Baja | Fin de suscripción; no elimina deudas |
| Borrado / Baja lógica | Marcar como dado de baja conservando histórico |
| Anonimización | Eliminar datos identificativos conservando el resto |
| Aprobación / Aprobación económica | Validación de operación sensible por Admin o Superadmin |
| Separación de funciones | Quien solicita no puede aprobar su propia solicitud sobre umbral |
| Auditoría | Registro no modificable de acciones y autor |
| Planificador | Componente que lanza procesos programados |
| Simulado | Componente que imita un servicio real para pruebas |
| Stripe | Plataforma de pagos de Turbine |
| API | Interfaz para comunicación con otros sistemas |
| Dashboard / Cuadro de mando | Pantalla de indicadores financieros para administradores |
| Responsive | Diseño adaptado a tamaño de pantalla |
| CSV | Fichero de texto con datos separados por comas |
| Gestoría | Asesoría externa contable/fiscal |
| Verifactu | Sistema de verificación de la AEAT; fuera de alcance |
| Brain | Producto principal de Turbine; independiente de esta app |
| On-premises | Instalación en servidor local de la empresa |
| Superadmin | Rol con máximos privilegios |
| Admin | Rol administrativo sin operaciones exclusivas del Superadmin |
| Usuario operativo | Antiguo "Lector"; solo gestiona clientes asignados |
| Tarifa | Precio acordado por servicio o plan |

