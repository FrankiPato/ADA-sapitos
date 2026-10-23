# Requisitos

## 1. Contexto y alcance

### 1.1. Objetivo del sistema

Turbine es una empresa de inteligencia artificial en fase inicial que cuenta con un equipo de 7 personas [P033–P035]. Su modelo de negocio se basa en una suscripción
mensual cuyo importe depende del rango de facturación anual del cliente, es decir, cuanto mayor es la facturación de la empresa cliente, mayor es su cuota mensual
[P035–P036]. Además, a dicha suscripción se le podrán sumar servicios adicionales
(como instalaciones locales o consultoría) que se facturan de forma puntual [P037, P192].

Actualmente, la facturación y el cobro se realizan de forma manual. Cada factura y cada recordatorio de pago se elaboran uno a uno [P213], y es necesario comprobar cada mes qué facturas se han cobrado [P037]. Este modelo no es sostenible para los objetivos de la empresa, que aspira a alcanzar miles de clientes y a contar con un departamento administrativo dedicado [P040–P041, P171].

Por tanto, el objetivo del sistema es proporcionar una aplicación web de uso interno, independiente del producto Brain de Turbine [P092–P096], que permita al personal
administrativo registrar clientes, generar facturas puntuales y recurrentes, y realizar el seguimiento de los cobros de la forma más automatizada posible [P107].

Como bien dijo Alejandro Ruiz, fundador de Turbine, establece los siguientes criterios de éxito [P042–P048, A112]:

- Rapidez y sencillez: mínimo número de clics
- Movilidad: uso desde cualquier lugar y dispositivo (ordenador, tablet o móvil).
- Fluidez de la información: ningún dato relevante debe dejar de llegar a la persona correcta.
- Automatización confiable y explicable: debe poder saberse siempre por qué una
factura se ha enviado, cobrado o quedado pendiente.

### 1.2. Alcance

Descripción de qué responsabilidades corresponden al sistema y, cuando sea relevante,
qué elementos quedan explícitamente fuera de su alcance.

Dentro del alcance: 
- Gestión de los clientes de Turbine (empresas), sus personas de contacto y sususcripción.
- Creación de facturas puntuales y generación automática de las recurrentes, con su correspondiente PDF y numeración.
- Envío automático de facturas y recordatorios de pago por email o WhatsApp (simulados).
- Detección de cobros consultando Stripe (simulado), con posibilidad de añadir otras fuentes de pago en el futuro.
- Seguimiento de impagos, incidencias, pausas y bajas de clientes.
- Gestión de usuarios internos, roles y asignación de clientes.
- Notificaciones internas, dashboard financiero (pantalla resumen con los números del negocio) y otros informes.
- Configuración del sistema (cosas que puede cambiar el superadministrador, como cambiar el plazo de 5 a 7 días), registro de actividad (auditoría, Alejandro remarca: saber qué ocurrió) y copias de seguridad.
- API documentada para que otros sistemas puedan consultar información (por ejemplo al darse de baja para consultar alguna factura pendiente).

Fuera del alcance: 
- Integración con Brain: la aplicación es totalmente independiente del producto de Turbine [A01].
- El sistema no cobra, solo consulta si una factura está pagada [P196].
- Acceso de los clientes de Turbine: la aplicación es solo de uso interno y los clientes contactan con soporte por otros medios [P141, P147–P148].
- Chat propio: las comunicaciones se hacen por email o WhatsApp [P101].
- Integración con Verifactu: lo gestiona la gestoría de Turbine [P146].
- Fiscalidad internacional completa: solo se pide que los datos no se limiten a España [A08].
- Usuario para la gestoría: se le envía un paquete exportado con la información, ya que Alejandro no quería que tuviese un usuario en la aplicación ya que podrían ver todos los datos de los clientes y no es necesario [A103].
- Pagos fraccionados: Turbine no los ofrece como modalidad de pago [P175, A33].

### 1.3. Actores y partes interesadas

En la siguiente tabla se recogen las personas y sistemas que interactúan con la
aplicación (actores), así como aquellos que pese a no utilizar la aplicación directamente, se ven
afectados por su funcionamiento (partes interesadas).

| Actor / stakeholder | Tipo | Descripción | Intereses principales |
|---|---|---|---|
| Superadministrador | Actor principal | Usuario interno de Turbine con permisos de configuración del sistema. Será una persona o un grupo muy reducido | Configurar plazos de pago, recordatorios, tarifas, series de facturación y plantillas, y consultar el registro de actividad |
| Administrador | Actor principal | Usuario interno que gestiona todas la operaciones: clientes, facturas, usuarios, asignaciones y aprobacione | Tener una visión global de la facturación y los cobros y controlar las operaciones económicas importantes |
| Usuario operativo | Actor principal | Empleado del departamento administrativo que trabaja únicamente con los clientes que tiene asignados. (lo que llamamos lector) | Gestionar de forma rápida a sus clientes, sus facturas y sus cobros, y recibir los avisos que le corresponden |
| Planificador del sistema | Actor secundario (sistema) | Componente que lanza automáticamente los procesos, como la generación de facturas recurrentes del día 1, los recordatorios o las copias de seguridad (decisión tomada por el equipo ya que Alejandro pide tareas automáticas) | Que los procesos se ejecuten a tiempo, sin duplicados y dejando registro |
| Stripe (simulado) | Actor secundario (sistema externo) | Plataforma de pagos de Turbine, que informa de si una factura ha sido pagada o no. En el futuro podrían añadirse otras fuentes, como Revolut | Proporciona información sobre los cobros |
| Servicio de mensajería (simulado) | Actor secundario (sistema externo) | Servicio de email y WhatsApp encargado de entregar las facturas y los recordatorios a los clientes, e informar de si el envío ha fallado [P101, A26–A27] | Entregar las comunicaciones y avisar de rebotes o errores |
| Sistema externo vía API | Actor secundario (sistema externo) | Otro sistema de Turbine que consulta si un cliente tiene facturas pendientes o comunica una solicitud de baja | Consultar información de facturación sin saltarse las reglas del sistema (lo necesitamos por ejemplo para la gestión al dar de baja una empresa) |
| Cliente de Turbine | Parte interesada (no usa la aplicación) | Empresa a la que Turbine presta servicio. Recibe las facturas y recordatorios, pero no accede a la aplicación | Recibir sus facturas correctamente y tener un contacto de soporte ante cualquier problema o situacion |
| Gestoría | Parte interesada (no usa la aplicación) | Asesoría externa que lleva la contabilidad y la fiscalidad de Turbine. Recibe un paquete exportado con la información de facturación | Disponer de información contable completa y ordenada |
| Dirección de Turbine | Parte interesada | Fundador de la empresa (Alejandro Ruiz), que encarga el sistema | Reducir el trabajo manual y que la administración pueda crecer |

El superadministrador dispone de todas las capacidades del Administrador, además de la configuración avanzada del sistema y la consulta de la auditoría.
Los clientes de Turbine no se consideran actores del sistema, ya que nunca interactúan con la aplicación. Por ello, no aparecerán en el diagrama de casos de uso.


## 2. Requisitos de negocio

| ID | Requisito | Fuente |
|---|---|---|
| BR-01 | Reducir el trabajo manual de elaborar cada factura y redactar uno a uno los recordatorios de pago | Cliente / entrevista [P213,P214] |
| BR-02 | Automatizar el seguimiento de los cobros para detectar a tiempo los pagos pendientes y los posibles impagos | Cliente / entrevista [P040,P107] |
| BR-03 | Permitir que la administración pueda crecer (objetivo de Alejandro Ruiz), pasando de una sola persona a un departamento y de pocos clientes a miles, sin tener que rehacer el sistema | Cliente / entrevista [P040,P171,A03,A53] |
| BR-04 | Asegurar que la información importante (cobros, impagos, incidencias...) llega siempre a la persona que debe gestionarla | Cliente / entrevista [P047,A35] |
| BR-05 | Agilizar las tareas administrativas habituales, haciéndo estass más rápidas y sencillas| Cliente / entrevista [P043,P044] |
| BR-06 | Poder gestionar la administración desde cualquier lugar y dispositivo | Cliente / entrevista [P045,P046] |
| BR-07 | Conocer en todo momento la situación financiera de la empresa, es decir, lo facturado, lo cobrado y lo pendiente | Cliente / entrevista [P104, P130, A38] |
| BR-08 | Cumplir con las obligaciones de facturación y mantener la trazabilidad de los documentos emitidos | Cliente / entrevista [P083,P084,P085,P218,A14,A48] |
| BR-09 | Proteger los datos de los clientes | Cliente / entrevista [P219,A50,A51] |
| BR-10 | Mantener la gestión administrativa separada del producto Brain y preparada para cambios futuros| Cliente / entrevista [P096,A01,A119] |


## 3. Requisitos de usuario

En la columna "Actor", el término "Usuario interno" engloba a los tres roles de
Turbine (Superadministrador, Administrador y Usuario operativo).

| ID | Actor | Requisito | Fuente |
|---|---|---|---|
| UR-01 | Usuario interno | Poder acceder a la aplicación de forma segura con su cuenta corporativa de Turbine | Cliente / entrevista [P125,P138,P139,A43] |
| UR-02 | Administrador | Poder dar de alta y desactivar usuarios, y decidir qué clientes puede ver y gestionar cada uno | Cliente / entrevista [P118,P119,P147,A45] |
| UR-03 | Administrador, Usuario operativo | Poder registrar y mantener actualizados los clientes con sus datos fiscales y su suscripción | Cliente / entrevista [P137,P159,A04,A07] |
| UR-04 | Administrador, Usuario operativo | Poder importar clientes desde un fichero CSV sin riesgo de introducir datos erróneos | Cliente / entrevista [P141,A41] |
| UR-05 | Administrador, Usuario operativo | Poder crear facturas puntuales y dejar configurada la facturación recurrente para no tener que crearla a mano cada mes | Cliente / entrevista [P110,P111,A17,A22] |
| UR-06 | Administrador, Usuario operativo | Poder revisar una factura antes de emitirla y corregir errores sin alterar facturas ya emitidas | Cliente / entrevista [P084,A14,A24,A70] |
| UR-07 | Administrador, Usuario operativo | Que las facturas y los recordatorios de pago lleguen automáticamente a los clientes y saber si se han entregado correctament | Cliente / entrevista [P088,P111,A26,A27,A28] |
| UR-08 | Administrador, Usuario operativo | Saber en todo momento qué facturas están cobradas, pendientes o vencidas sin tener que comprobarlo a mano | Cliente / entrevista [P106,P107,P108,A31] |
| UR-09 | Administrador, Usuario operativo | Poder gestionar situaciones excepcionales de cobro, como disputas, incidencias o pagos con un importe distinto al de la factura | Cliente / entrevista [A29,A32,A33,A66] |
| UR-10 | Administrador, Usuario operativo | Poder gestionar las pausas, bajas y reactivaciones de clientes sin perder datos, ni las deudas pendientes... | Cliente / entrevista [P098,A19,A20,A21] |
| UR-11 | Usuario interno | Recibir avisos de lo que afecta a sus clientes y ver en una pantalla qué tareas tiene pendientes | Cliente / entrevista [P123, P199,P200,P201, A35,A36,A37] |
| UR-12 | Usuario interno | Poder encontrar rápidamente cualquier cliente o factura | Cliente / entrevista [P141,A40] |
| UR-13 | Administrador | Poder consultar la situación financiera de la empresa mediante un dashboard e informes, y exportarlos | Cliente / entrevista [P104,P130–P131,A38,A39] |
| UR-14 | Administrador, Superadministrador | Poder aprobar o rechazar las operaciones económicas que requieran control, como descuentos o importes elevados | Cliente / entrevista [A11–A12, A97] |
| UR-15 | Administrador | Poder revisar una vez al año el rango de tarifa de cada cliente a partir de su facturación anual | Cliente / entrevista [P114,P146,A10,A72] |
| UR-16 | Superadministrador | Poder configurar los parámetros del sistema (plazos de pago, recordatorios, tarifas, series, plantillas) sin necesidad de modificar el código | Cliente / entrevista [P134,P144,A09,A57] |
| UR-17 | Superadministrador | Poder consultar quién ha realizado cada acción y cuándo, y comprobar que los procesos automáticos funcionan correctamente | Cliente / entrevista [P207–P208,A48,A55,A77] |
| UR-18 | Sistema externo vía API | Poder consultar si un cliente tiene facturas pendientes y comunicar solicitudes de baja | Cliente / entrevista [P203,P212, A74,A75] |


## 4. Requisitos del sistema

### 4.1. Requisitos funcionales

#### Usuarios y acceso

| ID | Requisito | Relacionado con |
|---|---|---|
| FR-01 | El sistema deberá permitir el acceso a la app mediante email y contraseña. | UR-01 |
| FR-02 | El sistema deberá rechazar cualquier email que no pertenezca al dominio corporativo de Turbine | UR-01 |
| FR-03 | El sistema deberá permitir recuperar la contraseña mediante un enlace que caduque pasado un tiempo | UR-01 |
| FR-04 | El sistema deberá cerrar la sesión automáticamente tras un periodo de inactividad | UR-01 |
| FR-05 | El sistema deberá permitir al administrador crear, modificar y desactivar usuarios, sin opción de registrarse por cuenta propia. Al desactivar un usuario, se cerrarán sus sesiones y se conservará el registro de sus acciones | UR-02, UR-17 |
| FR-06 | El sistema deberá asignar a cada usuario uno de los tres roles: Superadministrador, Administrador o Usuario operativo | UR-02 |
| FR-07 | El sistema deberá permitir al administrador asignar y desasignar clientes a cada usuario operativo | UR-02 |
| FR-08 | El sistema deberá impedir que un usuario operativo acceda a clientes o facturas que no tenga asignados, incluso escribiendo directamente la dirección web | UR-02 |


#### Clientes y contactos

| ID | Requisito | Relacionado con |
|---|---|---|
| FR-09 | El sistema deberá permitir crear, consultar, modificar y dar de baja empresas cliente | UR-03 |
| FR-10 | El sistema deberá guardar de cada cliente su nombre comercial, razón social, identificador fiscal, dirección fiscal, país, moneda, sector y responsable interno | UR-03 |
| FR-11 | El sistema deberá avisar si ya existe un cliente con el mismo id fiscal | UR-03 |
| FR-12 | El sistema deberá conservar los datos de los clientes dados de baja en lugar de borrarlos | UR-03, UR-10 |
| FR-13 | El sistema deberá permitir asociar varias personas de contacto a una empresa, indicando su función (facturación, impagos...), y marcar un contacto como inactivo sin perder el registro de las facturas que se le enviaron | UR-03 |
| FR-14 | El sistema deberá notificar si se intenta eliminar el único contacto de facturación de un cliente que se encuentra activo | UR-03 |
| FR-15 | El sistema deberá mostrar el historial de cada cliente: alta, facturas, pagos, incidencias, cambios de tarifa y bajas | UR-03 |

#### Suscripción

| ID | Requisito | Relacionado con |
|---|---|---|
| FR-16 | El sistema deberá permitir registrar la suscripción de un cliente con su fecha de inicio, su tarifa mensual y sus condiciones de pago | UR-05 |
| FR-17 | El sistema deberá mostrar el importe prorrateado del primer mes antes de confirmar el alta | UR-05 |
| FR-18 | El sistema deberá permitir indicar una fecha de inicio futura, sin generar facturas hasta esa fecha | UR-05 |
| FR-19 | El sistema deberá permitir condiciones de pago particulares para un cliente (por ejemplo, a 30 días) que estarán por encima de las generales | UR-05 |
| FR-20 | El sistema deberá impedir activar una suscripción si al cliente le faltan datos fiscales obligatorios, indicando cuáles son los datos que faltan | UR-05 |

#### Facturas

| ID | Requisito | Relacionado con |
|---|---|---|
| FR-21 | El sistema deberá permitir crear facturas puntuales con una o varias líneas, cada una con concepto, cantidad, precio, descuento e impuesto | UR-05 |
| FR-22 | El sistema deberá ofrecer un catálogo de conceptos frecuentes con un precio orientativo que se pueda modificar al crear cada factura | UR-05 |
| FR-23 | El sistema deberá generar automáticamente (día 1 del mes) las facturas de todas las suscripciones activas | UR-05 |
| FR-24 | El sistema deberá generar, en la fecha de inicio de la suscripción, una factura proporcional por los días que quedan de ese mes | UR-05 |
| FR-25 | El sistema no deberá generar facturas a clientes pausados, pendientes de activación o dados de baja | UR-05, UR-10 |
| FR-26 | Si falla la generación de la factura de un cliente, el sistema deberá continuar con el resto y avisar al responsable de ese cliente | UR-05, UR-11 |
| FR-27 | El sistema deberá mostrar una vista previa de la factura antes de emitirla | UR-06 |
| FR-28 | El sistema deberá impedir emitir una factura si falta algún dato fiscal obligatorio, indicando cuál | UR-06 |
| FR-29 | El sistema deberá permitir modificar o eliminar una factura solo mientras esté en borrador | UR-06 |
| FR-30 | Al emitir una factura, el sistema deberá asignarle un número único y correlativo dentro de su serie | UR-05 |
| FR-31 | El sistema deberá generar el PDF de cada factura con la imagen corporativa de Turbine, los datos fiscales, las líneas, los impuestos, el total y los datos de soporte | UR-05 |
| FR-32 | El sistema deberá conservar el PDF de cada factura emitida sin cambios, aunque después se modifiquen la plantilla o el logo de la empresa | UR-06 |
| FR-33 | El sistema deberá permitir crear una factura rectificativa a partir de una factura emitida, manteniendo el vínculo entre ambas facturas | UR-06 |
| FR-34 | El sistema deberá permitir filtrar las facturas por estado, tipo (puntual o recurrente), cliente y fecha | UR-08, UR-12 |

#### Envío y recordatorios

| ID | Requisito | Relacionado con |
|---|---|---|
| FR-35 | El sistema deberá enviar automáticamente cada factura emitida, con su PDF, por email a los contactos de facturación del cliente y, además, por WhatsApp a los contactos que hayan dado su consentimiento al darse de alta | UR-07 |
| FR-36 | El sistema deberá incluir el email y el teléfono de soporte de Turbine en todas las comunicaciones a los clientes | UR-07 |
| FR-37 | El sistema deberá registrar cada envío con su canal, destinatario, fecha y resultado | UR-07 |
| FR-38 | Si un envío falla o el correo rebota, el sistema deberá avisar al responsable del cliente y permitir corregir el contacto y reenviar la factura sin crear una nueva | UR-07, UR-11 |
| FR-39 | El sistema deberá enviar automáticamente un recordatorio antes del vencimiento (el día 3 después de ser emitida, por ejemplo), otro el día del vencimiento y, a partir de ahí, recordatorios periódicos (cada X días) hasta que la factura se pague, calculando las fechas a partir del vencimiento de cada factura según las condiciones de pago del cliente | UR-07 |
| FR-40 | El sistema deberá dejar de enviar recordatorios en cuanto una factura quede pagada | UR-07, UR-08 |
| FR-41 | El sistema deberá usar plantillas de mensaje editables, en español e inglés, con datos como el nombre del cliente, el número de factura, el importe y el vencimiento | UR-07, UR-16 |

#### Cobros

| ID | Requisito | Relacionado con |
|---|---|---|
| FR-42 | El sistema deberá consultar periódicamente los pagos en Stripe (simulado) y recibir sus avisos de pago | UR-08 |
| FR-43 | El sistema deberá asociar cada pago recibido a su factura y marcarla como pagada cuando la coincidencia sea clara (misma referencia e importe) | UR-08 |
| FR-44 | El sistema deberá ignorar los avisos de pago repetidos, de forma que un mismo pago no se cuente dos veces | UR-08 |
| FR-45 | Si un pago no tiene una referencia clara o puede corresponder a varias facturas, el sistema deberá dejarlo pendiente para que un usuario lo asocie manualmente | UR-09 |
| FR-46 | Si el importe pagado es menor o mayor que el de la factura, el sistema deberá registrar la diferencia y dejarla pendiente de revisión y no marcar la factura como pagada | UR-09 |
| FR-47 | El sistema deberá permitir asociar manualmente un pago a una factura y deshacer una asociación incorrecta, registrando quién lo hizo y por qué | UR-09 |
| FR-48 | El sistema deberá calcular el saldo de cada cliente: total facturado, cobrado y pendiente. Debe tener en cuenta las facturas rectificativas obviamente | UR-08 |
| FR-49 | El sistema deberá permitir pausar los recordatorios de una factura en disputa, indicando el motivo y una fecha de revisión, y avisar al responsable cuando llegue esa fecha | UR-09, UR-11 |
| FR-50 | El sistema deberá permitir abrir incidencias internas sobre un cliente o una factura, con responsable, estado, comentarios y resolución, pudiendo estas pausar los recordatorios | UR-09 |
| FR-51 | El sistema deberá permitir a un administrador declarar una factura como incobrable, indicando el motivo y con aprobación, sin marcarla como pagada | UR-09, UR-14 |

#### Bajas, pausas y revisión anual

| ID | Requisito | Relacionado con |
|---|---|---|
| FR-52 | El sistema deberá permitir pausar la suscripción de un cliente con un motivo, una fecha de inicio y una fecha prevista de reactivación, y avisar al responsable cuando llegue esa fecha | UR-10, UR-11 |
| FR-53 | El sistema deberá permitir registrar la solicitud de baja de un cliente, ya sea manualmente o recibida a través de la API | UR-10, UR-18 |
| FR-54 | Al hacerse efectiva la baja, el sistema deberá dejar de generar nuevas facturas recurrentes a ese cliente | UR-10 |
| FR-55 | Si el cliente que se da de baja tiene facturas pendientes, el sistema deberá mantener los recordatorios de esas facturas hasta que se paguen | UR-10 |
| FR-56 | El sistema deberá permitir reactivar a un cliente dado de baja usando su ficha anterior, sin crear un duplicado | UR-10 |
| FR-57 | El sistema deberá mostrar una vez al año la lista de clientes cuyo rango de tarifa debe revisarse, y permitir al administrador introducir la facturación anual de cada uno, indicando la fuente y la fecha del dato, para proponer el rango que le corresponde | UR-15 |
| FR-58 | El cambio de tarifa deberá aprobarse antes de aplicarse, se aplicará a partir de una fecha y no modificará las facturas anteriores | UR-14, UR-15 |

#### Aprobaciones, notificaciones y pantalla de trabajo

| ID | Requisito | Relacionado con |
|---|---|---|
| FR-59 | El sistema deberá exigir la aprobación de un administrador para las operaciones económicas más sensibles (descuentos, cambios manuales de importe, facturas puntuales por encima de un importe configurado...), sin exigirla para las facturas recurrentes generadas a partir de una suscripción ya aprobada | UR-14 |
| FR-60 | El sistema deberá ofrecer una bandeja de aprobaciones donde el solicitante vea si su solicitud está pendiente, aprobada o rechazada, junto con el comentario de quien lo aprobó | UR-14 |
| FR-61 | El sistema deberá impedir que un usuario apruebe su propia solicitud cuando el importe supere el umbral configurado | UR-14 |
| FR-62 | El sistema deberá enviar notificaciones internas al responsable de cada cliente ante pagos recibidos, facturas vencidas, envíos fallidos, disputas, revisiones anuales y aprobaciones resueltas, y permitir marcarlas como leídas y como resueltas | UR-11 |
| FR-63 | El sistema deberá mostrar al entrar una pantalla de trabajo diario con las facturas que vencen pronto, las vencidas, los pagos pendientes de asociar, los envíos fallidos y las aprobaciones pendientes | UR-11 |

#### Dashboard, informes, búsqueda e importación

| ID | Requisito | Relacionado con |
|---|---|---|
| FR-64 | El sistema deberá mostrar solo a los administradores un dashboard con lo facturado, cobrado, pendiente y vencido por semana, mes, trimestre y año, el tiempo medio de cobro, los clientes con mayor deuda y la previsión de ingresos recurrentes del mes siguiente | UR-13 |
| FR-65 | El sistema deberá permitir pulsar en cada cifra del dashboard para ver las facturas que la componen | UR-13 |
| FR-66 | El sistema deberá permitir generar informes filtrados por periodo, cliente, responsable, país, sector, estado y tipo de factura | UR-13 |
| FR-67 | El sistema deberá permitir exportar los informes a CSV y PDF, incluyendo solo los datos que el usuario tenga permiso para ver | UR-13 |
| FR-68 | El sistema deberá generar un paquete mensual o trimestral para la gestoría con las facturas emitidas, las rectificativas, los cobros y sus PDF | UR-13 |
| FR-69 | El sistema deberá ofrecer una búsqueda global por nombre, razón social, identificador fiscal, persona de contacto, email o número de factura | UR-12 |
| FR-70 | El sistema deberá permitir filtrar los clientes por ubicación, sector y estado | UR-12 |
| FR-71 | El sistema deberá permitir importar clientes desde un fichero CSV, mostrando una vista previa antes de confirmar en la que se señalen las filas con errores o con identificadores fiscales duplicados, e importando solo las filas válidas | UR-04 |
| FR-72 | El sistema deberá guardar un resumen de cada importación con las filas creadas, las rechazadas y el motivo | UR-04 |

#### Configuración, auditoría y API

| ID | Requisito | Relacionado con |
|---|---|---|
| FR-73 | El sistema deberá permitir al superadministrador configurar el plazo de pago por defecto, la cadencia de recordatorios, el dominio de correo permitido, los datos fiscales de Turbine, el logo, las series de facturación, los impuestos, los rangos de tarifa y los umbrales de aprobación. Los cambios se aplicarán solo a partir de ese momento, sin modificar las facturas ya emitidas | UR-16 |
| FR-74 | El sistema deberá registrar en la auditoría las acciones importantes (altas y cambios de clientes y datos fiscales, emisión de facturas, aprobaciones, asociación de pagos, cambios de permisos y cambios de configuración) y permitir al superadministrador consultarla y filtrarla | UR-17 |
| FR-75 | El sistema deberá registrar cada ejecución de los procesos automáticos, con el número de elementos procesados, los fallos y su motivo, y mostrar una pantalla de estado con la última generación de facturas, los mensajes pendientes de enviar, los errores recientes y la última copia de seguridad | UR-17 |
| FR-76 | El sistema deberá ofrecer una API que permita consultar si un cliente tiene facturas pendientes | UR-18 |
| FR-77 | El sistema deberá permitir que un sistema externo comunique por la API una solicitud de baja, que el sistema registrará para gestionarla con sus propias reglas | UR-18 |
| FR-78 | La API deberá exigir autenticación y aplicar los mismos permisos y reglas que la aplicación | UR-18 |

#### Requisitos complementarios

| ID | Requisito | Relacionado con |
|---|---|---|
| FR-79 | El sistema deberá mostrar el estado de cada cliente en el listado de clientes, sin necesidad de abrir su ficha [A37, A95] | UR-12 |
| FR-80 | El sistema deberá permitir elegir el impuesto de cada línea de factura entre los impuestos configurados en el sistema [A08] | UR-05 |
| FR-81 | El sistema deberá permitir registrar excepciones comerciales de un cliente (descuentos temporales, meses gratuitos o precios pactados), que requerirán aprobación según RB-13 [A11] | UR-05, UR-14 |
| FR-82 | El sistema deberá permitir asociar una misma persona de contacto a varias empresas cliente [A06] | UR-03 |

### 4.2. Requisitos no funcionales

| ID | Categoría | Requisito | Criterio verificable |
|---|---|---|---|
| NFR-01 | Usabilidad | Las operaciones habituales deberán realizarse de forma rápida y con el mínimo número de clics [P043,P216] | Crear un cliente con su suscripción, o una factura puntual, se completa en menos de 1 minuto en una prueba con un usuario habitual |
| NFR-02 | Usabilidad | Un usuario que conozca el trabajo administrativo deberá poder usar la aplicación sin formación previa [P044,A113] | En una prueba, un usuario que no conoce la aplicación da de alta un cliente y su suscripción sin ayuda |
| NFR-03 | Usabilidad | Los mensajes de error deberán ser comprensibles para el usuario [A63] | Los errores indican el campo afectado y el motivo (por ejemplo, "falta el NIF") y nunca muestran códigos técnicos |
| NFR-04 | Usabilidad | La interfaz deberá ser coherente y pedir confirmación solo cuando sea necesario [A114,A115] | Los botones principales están siempre en la misma posición, y solo se pide confirmación en acciones irreversibles o que afectan a varios elementos |
| NFR-05 | Portabilidad | La aplicación deberá poder usarse desde ordenador, tablet y móvil [P045,P046,P141,P217] | Es una aplicación web responsive cuyas pantallas se adaptan correctamente a móviles de 360 px de ancho sin desplazamiento horizontal |
| NFR-06 | Compatibilidad | La aplicación deberá funcionar en los navegadores actuales y sus ficheros exportados deberán leerse correctamente [A86] | Funciona en las versiones actuales de los navegafores más usados, y en los navegadores de Android e iOS; los CSV conservan tildes y caracteres especiales al abrirlos, y los PDF se abren con cualquier lector |
| NFR-07 | Accesibilidad | La interfaz deberá ser accesible [A62] | Todos los campos tienen etiqueta, el contraste es suficiente, se puede usar con teclado |
| NFR-08 | Rendimiento | Las búsquedas y la apertura de fichas deberán ser inmediatas [A52] | Con miles de clientes, una búsqueda o la apertura de una ficha tarda menos de 2 segundos en una conexión normal |
| NFR-09 | Rendimiento | Las tareas pesadas no deberán bloquear la aplicación [A52,A99] | La generación de muchas facturas o de informes grandes se hace en segundo plano y el usuario puede seguir trabajando mientras tanto |
| NFR-10 | Escalabilidad | El sistema deberá soportar el crecimiento de Turbine [P170,P171,A53] | Funciona con miles de clientes, cientos de miles de facturas y decenas de usuarios; los listados se cargan por páginas y no todos los datos a la vez |
| NFR-11 | Fiabilidad | Las operaciones y los procesos automáticos no deberán duplicar resultados [A15,A54,A100] | Repetir la generación mensual, un envío o la recepción de un pago, o pulsar dos veces el mismo botón, no crea facturas, números ni cobros duplicados; si el sistema está caído el día 1, al volver genera las facturas pendientes una única vez |
| NFR-12 | Fiabilidad | El fallo de un elemento no deberá detener un proceso completo [A55,A123] | Si la factura de un cliente falla en la generación mensual, las del resto de clientes se generan igualmente y el error queda registrado |
| NFR-13 | Consistencia | El sistema deberá funcionar correctamente cuando varias personas trabajen a la vez y dos facturas emitidas a la vez nunca reciben el mismo número [A47] | Si dos usuarios editan la misma factura, el segundo recibe un aviso en lugar de sobrescribir los cambios |
| NFR-14 | Exactitud | Los importes deberán calcularse sin errores de redondeo [A94] | Los importes se guardan con 2 decimales exactos, y el PDF, la base de datos y el dashboard muestran siempre las mismas cifras |
| NFR-15 | Seguridad | Solo los usuarios autenticados y con permiso deberán acceder a la información [A44,A50,A81] | Cualquier acceso sin sesión o a un recurso sin permiso es rechazado, también cuando se escribe directamente la dirección web o se usa la API |
| NFR-16 | Seguridad | Las contraseñas, las comunicaciones y los documentos deberán estar protegidos [A50] | Las contraseñas nunca se guardan en texto plano, todas las comunicaciones van cifradas (HTTPS), el repositorio no contiene claves ni contraseñas, y las facturas solo se descargan con sesión iniciada y permiso, o mediante enlaces que caducan |
| NFR-17 | Seguridad | El sistema deberá protegerse frente a intentos de acceso repetidos [A82] | Tras varios intentos fallidos seguidos, el acceso a esa cuenta se bloquea temporalmente |
| NFR-18 | Seguridad | El sistema deberá permitir añadir doble factor de autenticación en el futuro [P205,A43] | No se exige en esta versión, pero el diseño del acceso permite incorporarlo sin rehacerlo |
| NFR-19 | Privacidad | Los datos personales y fiscales de los clientes deberán protegerse desde el diseño [A51] | Solo se guardan los datos necesarios para facturar, no se usan para marketing, y los datos de un contacto pueden anonimizarse sin invalidar las facturas emitidas |
| NFR-20 | Trazabilidad | La automatización deberá ser explicable [A112] | De cada factura pagada se puede consultar el pago que lo justifica y de cada envío fallido, el motivo del fallo |
| NFR-21 | Auditoría | El registro de actividad no deberá poder modificarse [A48] | Ningún usuario puede editar ni borrar registros de la auditoría |
| NFR-22 | Recuperabilidad | Los datos deberán poder recuperarse ante un fallo [A49] | Se hace una copia de seguridad automática diaria y se ha comprobado al menos una vez que se puede restaurar |
| NFR-23 | Internacionalización | El sistema deberá estar preparado para clientes de otros países [A07,A60,A61] | Las comunicaciones a clientes están disponibles en español e inglés, las fechas se guardan con su zona horaria y los datos fiscales no asumen el formato español |
| NFR-24 | Interoperabilidad | Las integraciones externas deberán poder sustituirse fácilmente [A32,A76] | Cambiar Stripe simulado por el real, o añadir otra fuente de pago, no obliga a modificar el resto del sistema |
| NFR-25 | Interoperabilidad | La API deberá estar documentada [P128,A74] | Existe una documentación de la API con todas sus operaciones, datos de entrada y respuestas |
| NFR-26 | Mantenibilidad | El sistema deberá poder mantenerlo otro equipo [A116] | El código sigue unas convenciones documentadas |
| NFR-27 | Mantenibilidad | Las reglas de negocio deberán estar probadas [A83] | Existen pruebas automáticas del prorrateo, la generación mensual, los vencimientos, los impuestos, los permisos y la conciliación de pagos |
| NFR-28 | Despliegue | El sistema deberá usar software libre y poder desplegarse de forma reproducible [P147,A85] | Todas las tecnologías son libres, la configuración está separada del código y existe una guía para poner en marcha el sistema |
| NFR-29 | Privacidad | Las demostraciones no deberán usar datos reales [A104] | El sistema incluye datos ficticios para pruebas |
| NFR-30 | Determinismo | Las funciones principales no deberán depender de inteligencia artificial [P078,A117] | La facturación, los vencimientos y los cobros funcionan con reglas fijas por tanto cualquier función de IA es opcional y desactivable |

## 5. Reglas de negocio

| ID | Regla |
|---|---|
| RB-01 | Un cliente es siempre una empresa, y debe tener al menos un contacto de facturación activo para poder emitirle facturas [P147,A05] |
| RB-02 | La tarifa mensual de un cliente depende de su rango de facturación anual y se mantiene igual durante todo el año. Los rangos y sus precios se configuran en el sistema [P035,P181,P182,A09] |
| RB-03 | El rango de tarifa se revisa una vez al año. El nuevo precio se aplica desde su fecha de efecto y nunca modifica facturas anteriores [P146,A10,A72] |
| RB-04 | Las facturas recurrentes son mensuales y se emiten el día 1 de cada mes [P156,P212] |
| RB-05 | El importe de la factura proporcional se calcula como: tarifa mensual × días que quedan de mes (incluido el día de inicio de la suscripción) entre los dias que tiene ese mes. Si la suscripción empieza el último día del mes, se factura solo ese día [P212,A17] |
| RB-06 | El plazo de pago por defecto es de 5 días naturales desde la emisión de la factura. Si un cliente tiene condiciones de pago pactadas, se aplican las suyas [P088,P144,A28] |
| RB-07 | Por defecto, los recordatorios se envían así (sugerencia Alejandro): la factura el día de emisión, un recordatorio el día 3, otro el día del vencimiento y, a partir de entonces, uno cada 3 días hasta que se pague. Estos plazos se pueden configurar [P088,P158,A28] |
| RB-08 | Un impago nunca provoca la cancelación del servicio ni una sanción: se siguen enviando recordatorios hasta que la factura se pague o una persona intervenga [P122,A28] |
| RB-09 | Una factura puede pasar por los siguientes estados: borrador, pendiente de aprobación, emitida, enviada, vencida y pagada. Además, puede quedar en envío fallido, en disputa, con diferencia de importe pendiente de revisión, rectificada, anulada o incobrable [A13] |
| RB-10 | Una factura solo puede modificarse o eliminarse mientras está en borrador. Una vez emitida no se modifica, y cualquier corrección se hace mediante una factura que rectifica la anterior [A14,A16,A70] |
| RB-11 | Cada factura emitida tiene un número único y correlativo dentro de su serie. El número se asigna al emitirla y reenviar una factura no cambia su número [P218,A15] |
| RB-12 | Una factura solo se considera pagada cuando se ha recibido el cobro de su importe total. Si se recibe un importe menor o mayor, la diferencia queda pendiente de revisión. Una factura incobrable, anulada o rectificada nunca se marca como pagada [P175, A30, A33] |
| RB-13 | Necesitan aprobación: los descuentos, los cambios manuales de importe, las facturas puntuales que superen un importe configurado, las rectificativas, declarar una factura incobrable y los cambios de tarifa. Las facturas recurrentes normales no necesitan aprobación [A11–A12, A30, A70] |
| RB-14 | Cuando un cliente se da de baja, se dejan de generar sus facturas recurrentes desde la fecha de baja, pero las facturas que tenga pendientes se siguen reclamando hasta que se paguen [A20] |
| RB-15 | Un cliente en pausa no genera facturas nuevas, pero sus facturas anteriores siguen su proceso de cobro normal [A19] |
| RB-16 | Los importes se calculan con 2 decimales. Cada línea se redondea al céntimo más cercano (redondeo hacia arriba en caso de empate) y el total es la suma de las líneas ya redondeadas [A94] |
| RB-17 | Los cambios de configuración (plazos, tarifas, plantillas o logo) solo afectan a las facturas nuevas, nunca a las ya emitidas [A09,A25,A57] |
| RB-18 | El Superadministrador puede hacer todo lo que hace el Administrador, además configura el sistema y consulta la auditoría. El Administrador gestiona todos los clientes, facturas, usuarios y aprobaciones. El Usuario operativo solo puede consultar y modificar los clientes que tiene asignados y sus facturas, y no puede asignarse clientes, cambiar tarifas, crear series ni ver el dashboard. En los diagramas de casos de uso esto se representa como una generalización: el Superadministrador hereda del Administrador, y este del Usuario operativo [P149,P151,A44] |
| RB-19 | Los datos de los clientes no se borran: se conservan el tiempo que permita la ley. Los datos personales de un contacto pueden anonimizarse cuando proceda, pero las facturas emitidas se conservan siempre [P100,P112,A51,A110] |
| RB-20 | Un cliente puede estar en uno de estos estados: pendiente de activación, activo, pausado, baja solicitada o inactivo. Solo los clientes activos generan facturas recurrentes [A18,A19,A20,A95] |
| RB-21 | Los informes y exportaciones generados se conservan durante un tiempo configurable, mientras que las facturas emitidas se conservan según la obligación legal [A109,A110] |


## 6. Suposiciones y decisiones de análisis

| ID | Suposición o decisión | Justificación | Requisitos afectados |
|---|---|---|---|
| A-01 | Se definen tres roles: Superadministrador, Administrador y Usuario operativo. Al antiguo "lector" se le llama Usuario operativo | El cliente fue cambiando los roles durante la primera sesión [P119,P147,P149,P151] y en la ampliación aclaró que importa lo que puede hacer cada uno, no el nombre [A44]. "Lector" daba a entender que solo podía consultar, cuando también puede editar | FR-06, RB-18 |
| A-02 | Cuando el cliente responde que la aplicación no puede crear clientes [P209], se interpreta que se refiere a que un cliente no puede registrarse por sí mismo | Contradice el resto de la entrevista, donde los empleados sí crean clientes [P107, P159], y antes había dicho que "un cliente no se puede registrar a sí mismo" [P141] | FR-05, FR-09 |
| A-03 | En lugar de permitir editar facturas hasta el cambio de trimestre [P084–P085], solo se pueden editar los borradores por tanto, las facturas emitidas se corrigen con una factura nueva que rectifica la anterior | El cliente lo matiza así en la ampliación [A14] | FR-29, FR-33, RB-10 |
| A-04 | Los estados de la factura de RB-09 son una propuesta del equipo | El cliente pidió expresamente que analizáramos el ciclo de vida real de una factura y no copiáramos su lista [A13] | RB-09 |
| A-05 | La factura proporcional se calcula con los días reales del mes, no con meses de 30 días, e incluye el día de inicio | El cliente pidió que decidiéramos una regla coherente y la documentáramos [A17]. Con días reales, el caso del último día del mes no necesita un tratamiento especial | FR-24, RB-05 |
| A-06 | A partir del vencimiento, los recordatorios se envían cada 3 días | El cliente dijo "cada dos o tres días" [P158]. Elegimos 3 para no resultar insistentes. Se puede cambiar en la configuración | FR-39, RB-07 |
| A-07 | El dashboard y los informes muestran datos por semana, mes, trimestre y año | En la primera sesión se pidió primero "semanal, mensual, anual" [P104] y después "semanal, mensual y trimestral" [P130]. Incluimos los cuatro periodos, como confirma la ampliación [A38] | FR-64 |
| A-08 | Stripe, el email y WhatsApp se sustituyen por versiones simuladas. El email y WhatsApp se representan como un único actor, "Servicio de mensajería", ya que ambos cumplen la misma función que es entregar los mensajes y comunicar si el envío ha tenido éxito | El cliente autoriza expresamente la simulación [P147,A26,A31, A76] y pide registrar el resultado de cada envío [A26,A27,A105]. Unificar ambos canales en un actor simplifica el diagrama sin perder información | FR-35, FR-37, FR-38, FR-42, NFR-24 |
| A-09 | La facturación anual de cada cliente para la revisión de tarifa la introduce manualmente un administrador, indicando la fuente y la fecha del dato. La consulta automática a una fuente externa se deja como posible mejora futura | El cliente acepta las dos opciones [A72]. La entrada manual simplifica el sistema sin perder trazabilidad | FR-57 |
| A-10 | El importe se puede modificar al crear una factura puntual. En las recurrentes, el precio viene de la suscripción y solo cambia con la revisión anual o con una excepción aprobada | El cliente dijo que el importe debía poder modificarse cuando fuese [P144], pero también que el precio es el mismo todo el año [P180–P181]. La ampliación lo aclara así [A10,A11,A12] | FR-21, FR-23, RB-02, RB-13 |
| A-11 | Cuando un pago coincide claramente con una factura (misma referencia e importe), se asocia automáticamente. En cualquier otro caso se deja para revisión manual | El cliente permite que la conciliación se haga automáticamente o solo se proponga [A32]. Elegimos automatizar solo los casos claros para no marcar una factura equivocada | FR-43, FR-45 |
| A-12 | El importe a partir del cual una factura puntual necesita aprobación, y el umbral de separación de funciones, son configurables. Como valores iniciales se proponen 2.000 € y 10.000 € | El cliente pidió que fueran configurables [A11,A97] | FR-59, FR-61 |
| A-13 | Una búsqueda o la apertura de una ficha debe tardar menos de 2 segundos | El cliente no quiso dar una cifra exacta [A52]. Elegimos 2 segundos como límite razonable de algo casi inmediato como el comentò | NFR-08 |
| A-14 | Se hace una copia de seguridad automática diaria | El cliente pidió copias automáticas y probadas, pero no indicó la frecuencia [A49] | NFR-22 |
| A-15 | Las copias de seguridad no se modifican para borrar a un cliente concreto. Si se restaura una copia, se vuelven a aplicar las anonimizaciones que se hubieran hecho | El cliente reconoció que no lo tenía pensado [P187] y pidió una política para ello [A51] | RB-19 |
| A-16 | Cada línea de factura se redondea al céntimo, con redondeo hacia arriba en caso de empate | El cliente pidió que la regla de redondeo estuviera documentada [A94] | RB-16, NFR-14 |
| A-17 | El paquete para la gestoría se exporta como un archivo comprimido con un CSV y los PDF de las facturas | El cliente dejó el formato abierto [A102] | FR-68 |
| A-18 | Se incluye un actor "Planificador del sistema" que representa el paso del tiempo y lanza las tareas automáticas (facturas recurrentes, recordatorios, consulta de pagos y copias de seguridad) | El cliente pide que estas tareas se realicen automáticamente, sin intervención de ninguna persona. En UML, todo caso de uso debe ser iniciado por un actor y así vamos a representar los procesos que se inician por fecha u hora | 1.3, FR-23, FR-39, FR-42, NFR-22 |
| A-19 | En la importación de clientes desde CSV, se importan solo las filas válidas; las filas con errores o con identificador fiscal duplicado se rechazan y aparecen en el resumen, para que el usuario pueda corregirlas | El cliente deja la política a elección del equipo [A41]. Así no se bloquea toda la importación por unas pocas filas erróneas, y no entran datos incorrectos | FR-71, FR-72 |
| A-20 | Quedan fuera de esta versión las ampliaciones que el cliente presentó como opcionales: presupuestos, varias sociedades emisoras, centros de coste, conversión entre monedas, objetivos de facturación, cierre de periodos, fusión de clientes duplicados, importación de facturas antiguas, un futuro rol externo para la gestoría y funciones de inteligencia artificial | El cliente las presentó como ampliaciones o mejoras opcionales y pidió que lo que se deja fuera se documente por eso está aquí [A119] | — |
| A-21 | Se aplazan a versiones posteriores algunas funciones secundarias que el cliente mencionó, para priorizar el núcleo del sistema (clientes, facturación, envíos y cobros): delegación temporal de clientes, adjuntos de documentos, etiquetas de clientes, favoritos y filtros guardados, acciones masivas, menciones en comentarios, informes programados, alertas configurables, preferencias de canal de las notificaciones, calendario laboral y festivos, reasignación de aprobaciones a un suplente, notas restringidas solo para administradores, envío de pruebas de plantillas y la excepción comercial de no aplicar prorrateo | Son funciones de comodidad o de organización que no impiden el funcionamiento principal del sistema. Se priorizan las funciones imprescindibles para la primera versión, tal como permite el cliente [A119,A120] | - |

## 7. Dudas, ambigüedades y cuestiones pendientes

| ID | Cuestión | Origen | Impacto | Estado / resolución |
|---|---|---|---|---|
| Q-01 | ¿Puede darse de baja un cliente que tiene facturas pendientes? En la primera sesión se dice que no hasta que las pague [P098, P154, P168–P179]; en la ampliación, que sí, cerrando la recurrencia y manteniendo el cobro de lo pendiente [A20] | Entrevista y ampliación | Alto: afecta a la gestión de bajas y a los estados del cliente | Resuelta provisionalmente: se aplica lo indicado en la ampliación, por ser posterior y más detallada (RB-14, FR-54, FR-55). Se confirmará con el cliente |
| Q-02 | ¿La baja de un cliente la gestiona esta aplicación o un sistema externo? [P098, P202–P203] | Entrevista | Medio | Resuelta: la baja del producto se hace fuera; esta aplicación la registra y gestiona su efecto en la facturación (FR-53, FR-77) [A20, A75] |
| Q-03 | ¿Cuántos roles hay y qué puede hacer el "lector"? Primero solo consulta, después puede editar, y más tarde aparece un superadministrador [P119, P147, P149, P151] | Entrevista | Alto: afecta a todos los permisos | Resuelta: ver A-01 |
| Q-04 | ¿Se pueden editar las facturas hasta el cambio de trimestre? [P084–P085] | Entrevista | Alto: afecta a la validez legal de las facturas | Resuelta: ver A-03 |
| Q-05 | El cliente responde que la aplicación no puede crear un cliente [P209], lo que contradice el resto de la entrevista | Entrevista | Medio | Resuelta: ver A-02 |
| Q-06 | ¿Qué periodos deben mostrar los informes? Se dice "semanal, mensual, anual" [P104] y después "semanal, mensual y trimestral" [P130] | Entrevista | Bajo | Resuelta: ver A-07 |
| Q-07 | ¿El importe de una factura se puede modificar siempre [P144] o el precio es fijo todo el año [P180–P181]? | Entrevista | Medio | Resuelta: ver A-10 |
| Q-08 | ¿Los recordatorios de impago se envían cada dos o cada tres días? [P158] | Entrevista | Bajo (es configurable) | Resuelta: ver A-06 |
| Q-09 | ¿Una factura puntual pendiente impide la baja? [P168–P169] | Entrevista | Medio | Resuelta junto con Q-01: no impide la baja, pero se sigue reclamando |
| Q-10 | ¿Cuántos rangos de facturación anual hay y qué precio tiene cada uno? Solo se conoce el ejemplo de una empresa que factura 4 millones y paga 890 € al mes [P182] | Entrevista | Bajo (los rangos son configurables) | Pendiente: solicitar a Turbine la tabla de tarifas actual |
| Q-11 | ¿Durante cuánto tiempo deben conservarse los datos de los clientes? El cliente responde "el que me permita la ley" [P100, P185–P186] | Entrevista | Medio | Pendiente: lo debe confirmar la gestoría. Se deja como parámetro configurable |
| Q-12 | ¿Qué campos exactos debe tener el PDF de la factura? Se pidió una factura anonimizada de ejemplo [P146] | Entrevista | Medio: afecta al diseño del PDF | Pendiente de recibir la factura de ejemplo |
| Q-13 | ¿Qué datos fiscales son obligatorios para poder emitir una factura, sobre todo en clientes de otros países? [A07–A08, A24] | Ampliación | Medio | Pendiente: confirmar con la gestoría. Inicialmente se exigen razón social, identificador fiscal y dirección fiscal [P137] |
| Q-14 | ¿Qué importes concretos deben activar la aprobación y la separación de funciones? [A11, A97] | Ampliación | Medio | Pendiente de confirmar: se proponen valores iniciales (ver A-12) |
| Q-15 | Al llegar la fecha de reactivación de una pausa, ¿se reactiva automáticamente o se pide confirmación? [A19] | Ampliación | Bajo | Resuelta: se avisa al responsable para que lo confirme (FR-52) |
| Q-16 | ¿Cómo se obtiene y registra el consentimiento de un contacto para recibir mensajes por WhatsApp? [A106] | Ampliación | Bajo | Pendiente: de momento, un usuario lo marca manualmente en la ficha del contacto |
| Q-17 | ¿El plazo de pago de 5 días se cuenta en días naturales o laborables? [P088, P144] | Entrevista | Medio: afecta al cálculo del vencimiento y de los recordatorios | Resuelta provisionalmente: se cuentan días naturales (RB-06). Se confirmará con el cliente |

## 8. Trazabilidad

### 8.1. Relación entre requisitos

| Requisito de negocio | Requisito de usuario | Requisito funcional / no funcional |
|---|---|---|
| BR-01 Reducir el trabajo manual de facturación y recordatorios | UR-05, UR-06, UR-07 | FR-16 a FR-33, FR-35 a FR-41, FR-80, FR-81 |
| BR-02 Automatizar el seguimiento de los cobros | UR-08, UR-09, UR-10 | FR-34, FR-42 a FR-56, NFR-20, NFR-30 |
| BR-03 Permitir que la administración crezca | UR-02, UR-04, UR-17 | FR-05 a FR-08, FR-71, FR-72, FR-75, NFR-09 a NFR-13, NFR-26, NFR-27 |
| BR-04 Que la información llegue a la persona correcta | UR-11, UR-14 | FR-26, FR-38, FR-49, FR-52, FR-59 a FR-63 |
| BR-05 Agilizar las tareas habituales | UR-03, UR-12 | FR-09 a FR-15, FR-34, FR-69, FR-70, FR-79, FR-82, NFR-01 a NFR-04, NFR-08 |
| BR-06 Gestionar desde cualquier lugar y dispositivo | UR-01 | FR-01 a FR-04, NFR-05 a NFR-07 |
| BR-07 Conocer la situación financiera | UR-13, UR-15 | FR-57, FR-58, FR-64 a FR-68, NFR-06, NFR-14 |
| BR-08 Cumplir las obligaciones de facturación y mantener la trazabilidad | UR-06, UR-17 | FR-29, FR-30, FR-32, FR-33, FR-74, NFR-13, NFR-21, NFR-22 |
| BR-09 Proteger los datos de los clientes | UR-01, UR-02 | FR-08, FR-12, NFR-15 a NFR-19, NFR-29 |
| BR-10 Mantener el sistema independiente y preparado para el cambio | UR-16, UR-18 | FR-73, FR-76 a FR-78, NFR-23 a NFR-25, NFR-28 |

### 8.2. Requisitos de usuario y requisitos funcionales

| Requisito de usuario | Requisitos funcionales |
|---|---|
| UR-01 Acceso seguro | FR-01 a FR-04 |
| UR-02 Gestión de usuarios y asignaciones | FR-05 a FR-08 |
| UR-03 Gestión de clientes | FR-09 a FR-15, FR-82 |
| UR-04 Importación de clientes | FR-71, FR-72 |
| UR-05 Creación de facturas puntuales y recurrentes | FR-16 a FR-26, FR-30, FR-31, FR-80, FR-81 |
| UR-06 Revisión y corrección de facturas | FR-27 a FR-29, FR-32, FR-33 |
| UR-07 Envío de facturas y recordatorios | FR-35 a FR-41 |
| UR-08 Estado de los cobros | FR-34, FR-40, FR-42 a FR-44, FR-48 |
| UR-09 Situaciones excepcionales de cobro | FR-45 a FR-47, FR-49 a FR-51 |
| UR-10 Pausas, bajas y reactivaciones | FR-12, FR-25, FR-52 a FR-56 |
| UR-11 Avisos y pantalla de trabajo | FR-26, FR-38, FR-49, FR-52, FR-62, FR-63 |
| UR-12 Búsqueda | FR-34, FR-69, FR-70, FR-79 |
| UR-13 Dashboard e informes | FR-64 a FR-68 |
| UR-14 Aprobaciones | FR-51, FR-58 a FR-61, FR-81 |
| UR-15 Revisión anual de tarifa | FR-57, FR-58 |
| UR-16 Configuración | FR-41, FR-73 |
| UR-17 Auditoría y procesos automáticos | FR-05, FR-74, FR-75 |
| UR-18 API para sistemas externos | FR-53, FR-76 a FR-78 |

### 8.3. Ejemplo de trazabilidad completa: baja de un cliente con deuda

| Fuente | Requisito de usuario | Requisitos funcionales | Regla de negocio | Decisión / duda |
|---|---|---|---|---|
| [P098, P176–P179, A20, A75] | UR-10, UR-18 | FR-53, FR-54, FR-55, FR-77 | RB-14 | Q-01, Q-02 |

En las siguientes entregas esta trazabilidad se ampliará con los casos de uso, el diseño y las pruebas correspondientes.

## 9. Glosario

| Término | Definición |
|---|---|
| Turbine | Empresa que encarga el sistema y lo utiliza internamente. En este documento no se denomina "cliente" para evitar confusiones |
| Cliente | Empresa a la que Turbine presta sus servicios y factura. No es usuario de la aplicación |
| Contacto | Persona asociada a un cliente, con una función concreta (facturación, impagos, etc.). Una misma empresa puede tener varios contactos |
| Usuario interno | Empleado de Turbine que utiliza la aplicación: Superadministrador, Administrador o Usuario operativo |
| Responsable interno | Usuario interno asignado a un cliente, que gestiona sus facturas y cobros y recibe sus avisos |
| Delegación | Asignación temporal de los clientes de un usuario a otro, por ejemplo durante unas vacaciones |
| Suscripción | Relación comercial mensual, sin permanencia, entre Turbine y un cliente. Es la que da origen a las facturas recurrentes |
| Rango de facturación | Intervalo de facturación anual del cliente que determina el precio de su suscripción |
| Factura recurrente | Factura mensual que el sistema genera automáticamente el día 1 a partir de una suscripción |
| Factura puntual | Factura por un servicio adicional (instalación, consultoría, etc.) que se crea manualmente |
| Factura proporcional (prorrateo) | Factura por los días que quedan de mes cuando un cliente se da de alta a mitad de mes |
| Borrador | Factura que todavía no se ha emitido y que, por tanto, se puede modificar o eliminar |
| Emisión | Momento en que una factura recibe su número definitivo y deja de poder modificarse |
| Serie | Secuencia de numeración de las facturas (por ejemplo, una para facturas ordinarias y otra para rectificativas) |
| Factura rectificativa | Factura que corrige o anula total o parcialmente una factura ya emitida, y que queda vinculada a ella |
| Vencimiento | Fecha límite para pagar una factura. Por defecto, 5 días desde su emisión |
| Recordatorio | Mensaje que se envía a un cliente sobre una factura pendiente. No debe confundirse con una factura recurrente |
| Impago | Situación de una factura que ha vencido sin haberse pagado |
| Disputa | Situación en la que el cliente cuestiona una factura; sus recordatorios se pausan hasta una fecha de revisión |
| Incobrable | Factura que, por decisión aprobada, se da por imposible de cobrar. No equivale a "pagada" |
| Conciliación | Proceso de asociar un pago recibido con la factura a la que corresponde |
| Saldo | Diferencia entre lo facturado a un cliente y lo que ha pagado |
| Pausa | Suspensión temporal de la suscripción de un cliente, sin darlo de baja |
| Baja | Fin de la suscripción de un cliente. No elimina las deudas que tenga pendientes |
| Borrado lógico | Marcar un dato como dado de baja en lugar de eliminarlo, de forma que se conserva el histórico |
| Anonimización | Eliminar los datos que permiten identificar a una persona, conservando el resto de la información |
| Aprobación | Validación por parte de un administrador o superadministrador de una operación económica sensible antes de que se aplique |
| Separación de funciones | Regla por la que una persona no puede aprobar su propia solicitud cuando supera un importe determinado |
| Auditoría | Registro no modificable de quién ha hecho cada acción importante en el sistema y cuándo |
| Planificador | Parte del sistema que lanza automáticamente los procesos programados, como la generación de facturas del día 1 |
| Simulado | Componente que imita el comportamiento de un servicio real (Stripe, email o WhatsApp) para poder probar el sistema sin usar el servicio real |
| Stripe | Plataforma de pagos online que utiliza Turbine |
| API | Interfaz que permite que otros sistemas se comuniquen con la aplicación de forma automática |
| Dashboard | Pantalla con indicadores financieros resumidos, disponible solo para administradores |
| Responsive | Diseño web que se adapta automáticamente al tamaño de la pantalla (ordenador, tableta o móvil) |
| CSV | Formato de fichero de texto con datos separados por comas, utilizado para importar y exportar información |
| Gestoría | Asesoría externa que lleva la contabilidad y la fiscalidad de Turbine |
| Verifactu | Sistema de verificación de facturas de la Agencia Tributaria. Queda fuera del alcance |
| Brain | Producto principal de Turbine. Es totalmente independiente de esta aplicación |
