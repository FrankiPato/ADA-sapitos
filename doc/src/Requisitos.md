# Requisitos

## 1. Contexto y alcance

### 1.1. Objetivo del sistema

El sistema tiene como objetivo proporcionar una aplicación para gestionar las empresas cliente de la empresa y la información relacionada con su facturación y sus pagos.

El sistema permitirá gestionar empresas cliente, generar y consultar facturas puntuales y recurrentes, realizar el seguimiento de los pagos, controlar los retrasos y proporcionar información e informes que faciliten la gestión administrativa.

Además, el sistema deberá integrarse con Stripe para validar los pagos realizados por las empresas cliente y deberá proporcionar diferentes niveles de acceso en función del rol de cada usuario.

### 1.2. Alcance

El sistema será responsable de:

- Gestionar las cuentas de los usuarios y sus permisos.
- Gestionar diferentes roles de usuario.
- Recoger y guardar información de un acuerdo/ficha comercial o suscripción.
- Gestionar la información de las empresas cliente.
- Gestionar las personas de contacto asociadas a las empresas cliente.
- Generar y gestionar facturas puntuales y recurrentes.
- Calcular la facturación proporcional de las empresas cliente que se incorporen durante un mes.
- Controlar el estado de las facturas y de los pagos.
- Integrarse con Stripe para iniciar y validar los pagos.
- Enviar recordatorios y notificaciones relacionados con los pagos.
- Generar informes sobre facturación y pagos.
- Proporcionar un cuadro de mando para usuarios administrativos autorizados.
- Permitir el acceso desde ordenador, tablet y dispositivos móviles.
- Garantizar la seguridad, integridad y conservación de la información.

La Primera Reunión no especifica funcionalidades ajenas a la gestión de empresas cliente, facturación, pagos, usuarios e información administrativa, por lo que dichas funcionalidades quedan fuera del alcance definido.

### 1.3. Actores y partes interesadas

| Actor / stakeholder | Descripción | Intereses principales |
|---|---|---|
| Superadmin | Usuario con los mayores privilegios de gestión del sistema | Gestionar el sistema y disponer de los mayores privilegios de administración |
| Admin | Usuario con funciones administrativas | Gestionar empresas cliente, facturas, pagos e información administrativa según sus permisos |
| Lector | Usuario con acceso principalmente a la consulta de información | Consultar información sin realizar operaciones críticas |
| Cliente | Usuario asociado a una empresa cliente que utiliza el sistema para consultar información relacionada con su propia empresa y sus facturas | Consultar sus facturas y recibir información relacionada con pagos y vencimientos |
| Empresa | Organización que utiliza el sistema para gestionar sus empresas cliente, facturación y pagos | Facilitar y mejorar la gestión administrativa y el seguimiento de los cobros |
| Empresa cliente | Empresa que mantiene una relación comercial con la Empresa y cuyos servicios, facturas y pagos se gestionan mediante el sistema | Consultar sus facturas y recibir información relacionada con pagos y vencimientos |
| Persona de contacto | Persona asociada a una empresa cliente que actúa como contacto para determinadas gestiones | Recibir o proporcionar información relacionada con la relación comercial y administrativa |
| Stripe | Sistema externo utilizado para gestionar y validar pagos | Proporcionar confirmación de los pagos realizados |

## 2. Requisitos de negocio

Los siguientes requisitos representan las necesidades y objetivos de negocio identificados en la Primera Reunión.

| ID | Requisito | Fuente |
|---|---|---|
| BR-01 | Gestionar las empresas cliente y la información necesaria para su facturación | Primera Reunión |
| BR-02 | Gestionar facturas puntuales y recurrentes | Primera Reunión |
| BR-03 | Generar mensualmente las facturas recurrentes correspondientes | Primera Reunión |
| BR-04 | Aplicar una facturación proporcional a las empresas cliente que se incorporen durante un mes | Primera Reunión |
| BR-05 | Realizar un seguimiento de los pagos y del estado de las facturas de cada empresa cliente | Primera Reunión |
| BR-06 | Controlar los plazos de pago y detectar los retrasos | Primera Reunión |
| BR-07 | Integrarse con Stripe para validar los pagos realizados | Primera Reunión |
| BR-08 | Impedir la baja de una empresa cliente cuando tenga facturas pendientes de pago o saldo adeudado | Primera Reunión |
| BR-09 | Proporcionar información e informes sobre facturación y pagos para facilitar la gestión administrativa | Primera Reunión |
| BR-10 | Permitir el uso del sistema desde ordenador, tablet y dispositivos móviles | Primera Reunión |
| BR-11 | Proporcionar una interfaz sencilla e intuitiva centrada en la gestión de empresas cliente, facturas y pagos | Primera Reunión |
| BR-12 | Gestionar la relación comercial con cada empresa cliente de forma independiente de sus facturas | Segunda Reunión |
| BR-13 | Permitir la revisión periódica de las tarifas según el rango de facturación anual de cada empresa cliente | Segunda Reunión |
| BR-14 | Permitir excepciones comerciales, como descuentos temporales, meses gratuitos y precios pactados para servicios adicionales | Segunda Reunión |
| BR-15 | Garantizar la trazabilidad y la integridad de las facturas emitidas | Segunda Reunión |

## 3. Requisitos de usuario

Los requisitos de usuario se han separado de los requisitos funcionales, expresándolos desde el punto de vista de las necesidades de los distintos usuarios.

| ID | Actor | Requisito | Fuente |
|---|---|---|---|
| UR-01 | Usuarios | Poder iniciar sesión mediante sus credenciales | Primera Reunión |
| UR-02 | Usuarios | Disponer de permisos diferentes según el rol que tengan dentro del sistema | Primera Reunión |
| UR-03 | Administrador | Poder registrar nuevas empresas cliente introduciendo sus datos fiscales y de contacto | Primera Reunión |
| UR-04 | Usuarios autorizados | Poder consultar la información de las empresas cliente y sus facturas | Primera Reunión |
| UR-05 | Usuarios autorizados | Poder buscar y filtrar facturas por empresa cliente, fecha y estado | Primera Reunión |
| UR-06 | Usuarios autorizados | Poder consultar el estado de los pagos | Primera Reunión |
| UR-07 | Clientes | Recibir avisos relacionados con el estado y vencimiento de las facturas de su empresa cliente | Primera Reunión |
| UR-08 | Usuarios con permisos suficientes | Poder consultar informes sobre facturación y pagos | Primera Reunión |
| UR-09 | Administrador | Disponer de un cuadro de mando con información sobre el estado de la facturación | Primera Reunión |
| UR-10 | Usuarios | Poder utilizar la aplicación desde ordenador y dispositivos móviles | Primera Reunión |
| UR-11 | Administrador | Poder desactivar el acceso de una persona que ya no pertenezca a la empresa cliente | Segunda Reunión |
| UR-12 | Usuarios | No poder acceder al sistema cuando su cuenta se encuentre inactiva | Segunda Reunión |
| UR-13 | Administrador | Poder asociar una misma persona de contacto a varias empresas cliente | Segunda Reunión |
| UR-14 | Usuarios autorizados | Poder consultar los destinatarios a los que se envió una factura en el momento de su envío | Segunda Reunión |
| UR-15 | Poder consultar la situación de las empresas cliente sin tener que acceder individualmente a cada una | Segunda Reunión |
| UR-16 | Poder consultar y gestionar los datos comerciales y fiscales de una empresa cliente | Segunda Reunión |
| UR-17 | Poder configurar las series de facturación y las tablas de tarifas | Segunda Reunión |
| UR-18 | Poder revisar el cálculo del prorrateo antes de confirmar el alta de una empresa cliente | Segunda Reunión |
| UR-19 | Poder aprobar o rechazar cambios económicos que requieran autorización | Segunda Reunión |

## 4. Requisitos del sistema

### 4.1. Requisitos funcionales

| ID | Requisito | Relacionado con |
|---|---|---|
| FR-01 | El sistema deberá permitir crear cuentas de usuario | UR-01, UR-02 |
| FR-02 | El sistema deberá permitir a los usuarios iniciar sesión mediante usuario y contraseña | UR-01 |
| FR-03 | El sistema deberá almacenar las contraseñas de forma segura | UR-01 |
| FR-04 | El sistema deberá denegar el acceso cuando el usuario introduzca credenciales incorrectas | UR-01 |
| FR-05 | El sistema deberá controlar el acceso a las funcionalidades según el rol del usuario | UR-02 |
| FR-06 | Los usuarios internos deberán utilizar cuentas asociadas al dominio corporativo establecido por la empresa | UR-01 |
| FR-07 | El sistema deberá contemplar los roles Superadmin, Admin, Lector y Cliente | UR-02 |
| FR-08 | El Superadmin tendrá los mayores privilegios de gestión del sistema | UR-02 |
| FR-09 | El Admin tendrá acceso a las funciones administrativas que le correspondan, sin acceso a las operaciones reservadas al Superadmin | UR-02 |
| FR-10 | El Lector tendrá acceso restringido principalmente a la consulta de información y a modificaciones que no impliquen operaciones críticas | UR-02 |
| FR-11 | El Cliente solo podrá acceder a la información y funcionalidades relacionadas con su propia empresa cliente y sus facturas | UR-02 |
| FR-12 | El sistema deberá impedir que un usuario acceda a funcionalidades para las que no tenga permisos | UR-02 |
| FR-13 | El sistema deberá permitir dar de alta nuevas empresas cliente | UR-03 |
| FR-14 | El sistema deberá permitir introducir el NIF de la empresa cliente | UR-03 |
| FR-15 | El sistema deberá permitir introducir el nombre de la empresa cliente | UR-03 |
| FR-16 | El sistema deberá permitir introducir el correo electrónico de la empresa cliente | UR-03 |
| FR-17 | El sistema deberá permitir consultar la información de las empresas cliente | UR-04 |
| FR-18 | El sistema deberá permitir modificar la información de una empresa cliente cuando el usuario disponga de permisos suficientes | UR-03, UR-04 |
| FR-19 | El sistema deberá permitir dar de baja a una empresa cliente cuando no existan facturas pendientes de pago o saldo adeudado | UR-03 |
| FR-20 | El sistema podrá permitir la importación masiva de empresas cliente mediante archivos CSV | UR-03 |
| FR-21 | El sistema deberá permitir generar facturas puntuales | UR-04 |
| FR-22 | El sistema deberá permitir generar facturas recurrentes | UR-04 |
| FR-23 | El sistema deberá generar las facturas recurrentes correspondientes al ciclo mensual el día 1 de cada mes | UR-04 |
| FR-24 | El sistema deberá calcular automáticamente el importe proporcional cuando una empresa cliente se incorpore durante el transcurso de un mes | UR-04 |
| FR-25 | El sistema deberá permitir consultar las facturas asociadas a cada empresa cliente | UR-04 |
| FR-26 | El sistema deberá permitir buscar y filtrar facturas por empresa cliente, intervalo de fechas y estado | UR-05 |
| FR-27 | El sistema deberá permitir modificar una factura pendiente de pago únicamente a los usuarios con permisos suficientes | UR-04 |
| FR-28 | El sistema no deberá permitir modificar una factura que haya sido abonada | UR-04 |
| FR-29 | El sistema deberá registrar si una factura está pendiente de pago | UR-06 |
| FR-30 | El sistema deberá registrar la confirmación de un pago | UR-06 |
| FR-31 | El sistema deberá detectar las facturas cuyo plazo de pago haya expirado sin haberse confirmado el pago | UR-06, UR-07 |
| FR-32 | El sistema deberá cambiar el estado de la factura cuando se considere impagada | UR-06, UR-07 |
| FR-33 | El sistema deberá integrarse con Stripe | BR-07 |
| FR-34 | El sistema deberá poder iniciar el proceso de cobro mediante Stripe | BR-07 |
| FR-35 | El sistema deberá recibir y registrar la confirmación del pago proporcionada por Stripe | BR-07 |
| FR-36 | El sistema deberá asociar cada pago con la factura correspondiente | UR-06 |
| FR-37 | El sistema deberá actualizar el estado de la factura cuando el pago haya sido validado correctamente | UR-06 |
| FR-38 | El sistema deberá enviar recordatorios de pago mientras la factura permanezca pendiente | UR-07 |
| FR-39 | El sistema deberá enviar los recordatorios de forma periódica durante el plazo establecido | UR-07 |
| FR-40 | El sistema deberá controlar el plazo de pago establecido en 5 días desde la emisión de la factura | BR-06 |
| FR-41 | El sistema deberá identificar como impagada una factura que supere dicho plazo sin confirmación del pago | BR-06 |
| FR-42 | El sistema deberá notificar al Administrador cuando una factura pase a estado de impago | UR-07 |
| FR-43 | El sistema deberá proporcionar un cuadro de mando destinado a los usuarios administrativos autorizados | UR-09 |
| FR-44 | El cuadro de mando deberá mostrar información relevante sobre facturación y pagos | UR-09 |
| FR-45 | El sistema deberá permitir generar informes sobre la actividad de facturación | UR-08 |
| FR-46 | El sistema deberá permitir generar informes con periodicidad mensual y anual | UR-08 |
| FR-47 | El acceso al cuadro de mando y a los informes deberá estar restringido según los permisos del usuario | UR-08, UR-09 |
| FR-48 | El sistema deberá permitir asociar varias personas de contacto a una misma empresa cliente | BR-01 |
| FR-48 | El sistema deberá permitir marcar como inactiva una persona de contacto que ya no pertenezca a la empresa cliente | UR-11 |
| FR-49 | El sistema no deberá permitir el inicio de sesión a las personas de contacto que se encuentren en estado inactivo | UR-12 |
| FR-50 | El sistema deberá permitir asociar una misma persona de contacto a varias empresas cliente | UR-13 |
| FR-51 | El sistema deberá conservar los destinatarios a los que se envió cada factura en el momento de su envío, independientemente de los cambios posteriores en las personas de contacto de la empresa cliente | UR-14 |
| FR-52 | El sistema deberá permitir registrar la fecha de inicio, el servicio o plan contratado, la tarifa acordada, el responsable comercial interno, las condiciones de pago y la documentación comercial asociada a cada empresa cliente | BR-12 |
| FR-53 | El sistema deberá permitir adjuntar documentos firmados externamente al registro comercial de una empresa cliente | BR-12 |
| FR-54 | El sistema deberá permitir almacenar el nombre comercial, la razón social, el identificador fiscal, la dirección fiscal, el país, la moneda y los datos de contacto de cada empresa cliente | UR-16 |
| FR-55 | El sistema deberá permitir seleccionar los impuestos de las facturas entre las opciones previamente configuradas | UR-16 |
| FR-56 | El sistema deberá permitir configurar los tramos de facturación anual y las tarifas correspondientes a cada tramo | BR-13 |
| FR-57 | El sistema deberá permitir al Superadmin modificar la tabla de tarifas sin alterar los importes de las facturas históricas | BR-13 |
| FR-58 | El sistema deberá detectar cuándo corresponde revisar la tarifa de un cliente y proponer un tramo según su facturación anual verificada | BR-13 |
| FR-59 | El sistema deberá registrar quién ha aprobado un cambio de tarifa, cuándo se ha aprobado y cuáles eran el precio anterior y el nuevo | BR-13 |
| FR-60 | El sistema deberá permitir solicitar cambios económicos, como descuentos o modificaciones de precios, y gestionar su aprobación según los permisos establecidos | BR-14 |
| FR-61 | El sistema deberá permitir configurar los umbrales y las condiciones que determinan si un cambio económico requiere la aprobación del Admin o del Superadmin | BR-14 |
| FR-62 | El sistema deberá mantener pendientes de aprobación las facturas que incluyan cambios económicos o condiciones excepcionales que requieran autorización, indicando el motivo de la aprobación pendiente | UR-19 |
| FR-63 | El sistema deberá gestionar los estados del ciclo de vida de una factura, incluyendo borrador, aprobada o lista para emitir, emitida, enviada, vencida, pagada, parcialmente conciliada, anulada y rectificada, según corresponda | BR-15 |
| FR-64 | El sistema deberá impedir la modificación silenciosa de facturas emitidas y mantener la trazabilidad de los cambios relevantes | BR-15 |
| FR-65 | El sistema deberá permitir gestionar las rectificaciones de facturas emitidas mediante facturas rectificativas o el procedimiento equivalente que corresponda | BR-15 |
| FR-66 | El sistema deberá asignar a cada factura emitida un número único dentro de su serie y evitar duplicidades incluso cuando se emitan varias facturas simultáneamente | BR-15 |
| FR-67 | El sistema deberá permitir al Superadmin configurar las series de facturación disponibles | UR-17 |
| FR-68 | El sistema deberá permitir eliminar borradores de facturas que todavía no se hayan emitido, bajo las restricciones establecidas y dejando registro de la operación cuando sea necesario | BR-15 |
| FR-69 | El sistema deberá permitir dar de baja lógicamente a empresas cliente conservando su historial, sus relaciones y sus registros asociados | BR-15 |
| FR-70 | El sistema deberá mostrar el importe prorrateado correspondiente al primer periodo de facturación antes de confirmar el alta de una empresa cliente | UR-18 |
| FR-71 | El sistema deberá permitir registrar excepciones comerciales en las que la facturación comience el mes siguiente sin aplicar prorrateo | BR-14 |
| FR-72 | El sistema deberá permitir reintentar el envío de una factura ya emitida sin generar otra factura ni asignarle un nuevo número | BR-15 |







### 4.2. Requisitos no funcionales

Los requisitos no funcionales se han reformulado para incluir, cuando es posible, un criterio que permita verificar su cumplimiento. La Primera Reunión define las categorías de seguridad, control de acceso, protección de contraseñas, multiplataforma, usabilidad, escalabilidad, concurrencia, rendimiento, disponibilidad, despliegue, integridad, mantenibilidad y retención.

| ID | Categoría | Requisito | Criterio verificable |
|---|---|---|---|
| NFR-01 | Seguridad | La información sensible, incluyendo credenciales, información fiscal, facturación y pagos, deberá almacenarse y transmitirse utilizando mecanismos de seguridad adecuados | Las credenciales y datos sensibles no deberán almacenarse ni transmitirse sin los mecanismos de protección definidos para el sistema |
| NFR-02 | Control de acceso | El sistema deberá utilizar un mecanismo de control de acceso basado en roles | Un usuario no deberá poder acceder a funcionalidades que no correspondan a su rol |
| NFR-03 | Seguridad | Las contraseñas no deberán almacenarse en texto plano | La información almacenada correspondiente a las contraseñas no deberá permitir obtener directamente la contraseña original |
| NFR-04 | Compatibilidad / Responsive | El sistema deberá visualizarse correctamente en móvil, tablet y ordenador | Las principales funcionalidades deberán poder utilizarse desde los tres tipos de dispositivo |
| NFR-05 | Usabilidad | La interfaz deberá ser sencilla, intuitiva y estar centrada en las funcionalidades principales | Las funcionalidades principales de empresas cliente, facturas y pagos deberán poder utilizarse sin conocimientos técnicos específicos |
| NFR-06 | Escalabilidad | El sistema deberá poder almacenar información de múltiples empresas cliente, facturas y pagos sin que el crecimiento normal de los datos impida el funcionamiento de las funcionalidades principales | El crecimiento de empresas cliente, facturas y pagos no deberá impedir el funcionamiento de las funcionalidades principales |
| NFR-07 | Concurrencia | El sistema deberá soportar el acceso simultáneo de varios usuarios sin provocar inconsistencias en la información almacenada | Las operaciones simultáneas no deberán generar datos inconsistentes |
| NFR-08 | Rendimiento | Las operaciones habituales de consulta, búsqueda y navegación deberán ejecutarse de forma fluida | Las operaciones habituales deberán poder ejecutarse sin bloquear las funciones básicas del sistema |
| NFR-09 | Rendimiento | Las operaciones que requieran un procesamiento elevado, como la generación de informes o facturas masivas, no deberán bloquear las funciones básicas del sistema | La generación de informes o facturas masivas no deberá impedir el uso de las funciones básicas |
| NFR-10 | Disponibilidad | El sistema deberá estar disponible durante el horario de funcionamiento establecido por la empresa, salvo durante tareas programadas de mantenimiento | El sistema deberá estar disponible durante el horario establecido excepto durante los mantenimientos programados |
| NFR-11 | Despliegue | El sistema deberá permitir su instalación y ejecución en un entorno de servidor local (on-premises) | La aplicación deberá poder instalarse y ejecutarse en un servidor local |
| NFR-12 | Integridad | El sistema deberá mantener la coherencia entre empresas cliente, facturas y pagos | Las operaciones no deberán producir registros inconsistentes entre empresas cliente, facturas y pagos |
| NFR-13 | Mantenibilidad | El sistema deberá estar estructurado de forma que sus componentes puedan mantenerse, corregirse y ampliarse sin afectar innecesariamente al resto de funcionalidades | Los cambios o correcciones de un componente no deberán requerir modificaciones innecesarias en el resto del sistema |
| NFR-14 | Retención | Los datos relacionados con empresas cliente, facturación y pagos deberán conservarse durante el periodo que corresponda conforme a las obligaciones legales y fiscales aplicables | La información deberá conservarse durante el periodo legal o fiscal que corresponda |
| NFR-15 | Independencia | La aplicación administrativa deberá ser independiente de Brain y de su interfaz, de forma que pueda seguir funcionando aunque Brain deje de estar disponible | La aplicación deberá poder utilizarse sin depender de Brain |
| NFR-16 | Usabilidad | La aplicación deberá permitir realizar las operaciones administrativas habituales con pocos pasos y sin navegación innecesaria | Las operaciones habituales deberán poder completarse sin recorrer pantallas innecesarias |
| NFR-17 | Integridad | El sistema deberá garantizar la unicidad de la numeración de las facturas incluso cuando se produzcan emisiones simultáneas | Las emisiones simultáneas no deberán generar números de factura duplicados |

## 5. Reglas de negocio

Estas reglas representan condiciones del dominio que condicionan el comportamiento del sistema.

| ID | Regla |
|---|---|
| RB-01 | Las facturas recurrentes deberán generarse mensualmente el día 1 de cada mes. |
| RB-02 | Cuando una empresa cliente se incorpore durante el transcurso de un mes, deberá facturarse proporcionalmente el periodo restante hasta finalizar dicho mes. |
| RB-03 | A partir del mes siguiente a la incorporación, la empresa cliente comenzará el ciclo normal de facturación el día 1. |
| RB-04 | Una empresa cliente no podrá darse de baja mientras tenga facturas pendientes de pago o saldo adeudado. |
| RB-05 | El plazo de pago establecido será de 5 días desde la emisión de la factura. |
| RB-06 | Una factura que supere el plazo establecido sin confirmación del pago deberá identificarse como impagada. |
| RB-07 | Una factura que haya sido abonada no podrá modificarse. |
| RB-08 | El acceso a las funcionalidades del sistema dependerá del rol y permisos del usuario. |
| RB-09 | Un usuario con rol Cliente solo podrá acceder a la información y funcionalidades relacionadas con su propia empresa cliente y sus facturas. |
| RB-10 | Las tarifas de los clientes deberán revisarse anualmente según los tramos de facturación anual establecidos |
| RB-11 | Los cambios de tarifa deberán requerir validación humana antes de aplicarse |
| RB-12 | Los cambios económicos que superen los umbrales o cumplan las condiciones configuradas deberán requerir la aprobación correspondiente antes de aplicarse |
| RB-13 | Las facturas recurrentes derivadas de una relación comercial aprobada deberán generarse automáticamente sin requerir una aprobación adicional |
| RB-14 | Una factura emitida no podrá eliminarse como si nunca hubiera existido |
| RB-15 | El número definitivo de una factura deberá asignarse de forma segura en el momento de su emisión y ser único dentro de su serie |
| RB-16 | El cálculo del prorrateo deberá seguir una regla documentada y coherente con la duración real del mes, incluidos los casos de alta el último día del mes |
| RB-17 | Las excepciones comerciales que permitan comenzar a facturar el mes siguiente sin prorrateo deberán quedar registradas explícitamente |

## 6. Suposiciones y decisiones de análisis

Se documentan aquí las decisiones que se pueden identificar en la Primera Reunión y que son necesarias para interpretar los requisitos.

| ID | Suposición o decisión | Justificación | Requisitos afectados |
|---|---|---|---|
| A-01 | Se considera que los roles del sistema serán Superadmin, Admin, Lector y Cliente | La Primera Reunión especifica expresamente estos cuatro roles | FR-07 a FR-12, NFR-02 |
| A-02 | Se considera que el plazo de pago es de 5 días naturales desde la emisión de la factura, a falta de una especificación más concreta | El documento establece un plazo de 5 días, pero no especifica si se trata de días naturales o laborables | FR-40, FR-41, RB-05, RB-06 |
| A-03 | La importación masiva mediante CSV se considera una funcionalidad opcional | La Primera Reunión utiliza explícitamente la expresión "podrá permitir" | FR-20 |
| A-04 | Se considera que la validación realizada mediante Stripe es suficiente para confirmar un pago en el sistema | El documento establece que Stripe proporcionará la confirmación del pago | FR-33 a FR-37 |
| A-05 | Se considera que los informes podrán generarse con periodicidad mensual y anual | La Primera Reunión establece expresamente ambas periodicidades | FR-45, FR-46 |
| A-06 | Se considera que una empresa cliente puede tener varias personas de contacto asociadas | La Primera Reunión contempla que una misma empresa puede tener distintas personas de contacto para diferentes gestiones, como contratación, administración o seguimiento de impagos | BR-01, FR-13 a FR-18 |
| A-07 | Se considera que una empresa cliente constituye una única entidad dentro del sistema, independientemente del número de personas de contacto asociadas | La Primera Reunión indica que no se debe duplicar una misma empresa cuando existan varias personas de contacto | BR-01, FR-13 a FR-19 |
| A-08 | La relación comercial de una empresa cliente se gestionará de forma independiente de sus facturas y no implicará necesariamente un contrato de duración determinada | Segunda Reunión | BR-12, FR-52 |
| A-09 | La gestoría se encargará de las particularidades fiscales complejas; la aplicación almacenará los datos necesarios para clientes internacionales sin implementar toda la lógica fiscal internacional | Segunda Reunión | FR-54, FR-55 |
| A-10 | Los tramos de facturación anual y sus tarifas serán configurables y no estarán definidos de forma fija en el código | Segunda Reunión | FR-56, FR-57 |
| A-11 | Los estados de las facturas representarán su ciclo de vida real; los estados indicados inicialmente no constituyen necesariamente una lista cerrada | Segunda Reunión | FR-63 |

## 7. Dudas, ambigüedades y cuestiones pendientes

| ID | Cuestión | Origen | Impacto | Estado / resolución |
|---|---|---|---|---|
| Q-01 | ¿Cuál será el medio utilizado para enviar las notificaciones y recordatorios de pago? | Primera Reunión | Afecta al mecanismo de notificaciones y a FR-38, FR-39 y FR-42 | Pendiente: el documento indica que deberá definirse durante el desarrollo |
| Q-02 | ¿Los 5 días de plazo de pago son naturales o laborables? | Primera Reunión | Afecta al cálculo del vencimiento y a la detección de impagos | Pendiente |
| Q-03 | ¿Qué se considera exactamente "saldo adeudado" a efectos de impedir la baja de una empresa cliente? | Primera Reunión | Afecta a la condición de baja de la empresa cliente | Pendiente |
| Q-04 | ¿Qué información concreta deberá mostrar el cuadro de mando? | Primera Reunión | Afecta al contenido de FR-43 y FR-44 | Pendiente |
| Q-05 | ¿Qué información concreta deberán contener los informes mensuales y anuales? | Primera Reunión | Afecta al contenido de FR-45 y FR-46 | Pendiente |
| Q-06 | ¿Qué operaciones están reservadas exclusivamente al Superadmin? | Primera Reunión | Afecta a la diferenciación de permisos entre Superadmin y Admin | Pendiente |
| Q-07 | ¿Qué modificaciones se consideran operaciones no críticas para el rol Lector? | Primera Reunión | Afecta a los permisos del rol Lector | Pendiente |
| Q-08 | ¿Qué información deberá almacenarse de cada persona de contacto y qué función o tipo de contacto tendrá? | Primera Reunión | Afecta al modelo de datos y a la gestión de las empresas cliente | Pendiente |
| Q-09 | ¿Cuál será la regla exacta de prorrateo y cómo se tratarán las altas realizadas el último día del mes? | Segunda Reunión | Afecta al cálculo de FR-24 y a RB-02 y RB-03 | Pendiente |
| Q-10 | ¿Cuáles serán los estados definitivos de las facturas y qué transiciones estarán permitidas entre ellos? | Segunda Reunión | Afecta al ciclo de vida de las facturas | Pendiente |
| Q-11 | ¿Qué condiciones concretas determinarán cuándo una factura o un cambio económico necesita aprobación? | Segunda Reunión | Afecta al flujo de aprobación de facturas y cambios económicos | Pendiente |
| Q-12 | ¿Qué restricciones se aplicarán a la eliminación de borradores y a la baja lógica de empresas cliente? | Segunda Reunión | Afecta a la eliminación y conservación del historial | Pendiente |

## 8. Trazabilidad

La siguiente tabla relaciona los objetivos de negocio con las necesidades de los usuarios y los requisitos del sistema.

| Requisito de negocio | Requisito de usuario | Requisito funcional / no funcional |
|---|---|---|
| BR-01 | UR-03, UR-04, UR-11, UR-12, UR-13, UR-14, UR-15 | FR-13 a FR-20, FR-48 a FR-51, FR-54, FR-69, NFR-06, NFR-12 |
| BR-02 | UR-04 | FR-21, FR-22, FR-25, FR-63 a FR-65, FR-68, FR-72 |
| BR-03 | UR-04 | FR-23, RB-01 |
| BR-04 | UR-04, UR-18 | FR-24, FR-70, FR-71, RB-02, RB-03, RB-16, RB-17 |
| BR-05 | UR-06 | FR-29 a FR-32, FR-36, FR-37, FR-63 |
| BR-06 | UR-07 | FR-31, FR-32, FR-38 a FR-42, RB-05, RB-06 |
| BR-07 | UR-06 | FR-33 a FR-37 |
| BR-08 | UR-03 | FR-19, FR-69, RB-04 |
| BR-09 | UR-08, UR-09, UR-15 | FR-43 a FR-47 |
| BR-10 | UR-10 | NFR-04 |
| BR-11 | UR-04, UR-05, UR-06, UR-08, UR-09 | NFR-05, NFR-08, NFR-09, NFR-16 |
| BR-12 | UR-16 | FR-52 a FR-55, A-08, A-09 |
| BR-13 | UR-17 | FR-56 a FR-59, RB-10, RB-11, A-10 |
| BR-14 | UR-19 | FR-60 a FR-62, FR-71, RB-12 |
| BR-15 | UR-17 | FR-63 a FR-69, FR-72, NFR-17, RB-14, RB-15 |
| — | UR-01 | FR-01 a FR-04, NFR-03 |
| — | UR-02 | FR-05 a FR-12, NFR-02 |
| — | UR-05 | FR-26 |
| — | UR-07 | FR-38 a FR-42 |
| — | UR-08 | FR-45 a FR-47 |
| — | UR-09 | FR-43, FR-44, FR-47 |
| — | UR-16 | FR-54, FR-55 |
| — | UR-18 | FR-70 |

## 9. Glosario

| Término | Definición |
|---|---|
| Empresa | Organización que utiliza el sistema para gestionar sus empresas cliente, facturación y pagos. |
| Empresa cliente | Empresa que mantiene una relación comercial con la Empresa y para la que se gestionan servicios, facturas y pagos en el sistema. |
| Persona de contacto | Persona asociada a una empresa cliente que puede desempeñar distintas funciones de contacto, como contratación, administración o seguimiento de impagos. |
| Factura puntual | Factura generada de manera individual y no asociada a un ciclo recurrente. |
| Factura recurrente | Factura que forma parte de un ciclo periódico de facturación mensual. |
| Facturación proporcional | Importe calculado para cubrir el periodo restante del mes cuando una empresa cliente se incorpora durante el transcurso del mismo. |
| Pago | Operación mediante la cual una empresa cliente abona el importe correspondiente a una factura. |
| Factura pendiente | Factura cuyo pago todavía no ha sido confirmado. |
| Factura impagada | Factura cuyo plazo de pago ha expirado sin que se haya confirmado el pago. |
| Superadmin | Rol de usuario con los mayores privilegios de gestión del sistema. |
| Admin | Rol de usuario con funciones administrativas, pero sin acceso a las operaciones reservadas al Superadmin. |
| Lector | Rol con acceso principalmente a la consulta de información y a modificaciones que no impliquen operaciones críticas. |
| Cliente (rol) | Rol que permite a un usuario acceder únicamente a la información y funcionalidades relacionadas con su propia empresa cliente y sus facturas. |
| Stripe | Sistema externo con el que se integra la aplicación para iniciar y validar pagos. |
| Cuadro de mando | Interfaz destinada a usuarios administrativos autorizados que muestra información relevante sobre facturación y pagos. |
| CSV | Formato de archivo que podrá utilizarse para la importación masiva de empresas cliente. |
| On-premises | Entorno en el que el sistema se instala y ejecuta en un servidor local de la empresa. |
| Relación comercial | Información que recoge las condiciones acordadas entre la empresa y una empresa cliente, incluyendo el servicio contratado, la tarifa y las condiciones de pago. |
| Datos comerciales | Información relativa a la relación comercial con una empresa cliente, como el servicio contratado, la tarifa acordada y el responsable comercial. |
| Datos fiscales | Información utilizada para identificar fiscalmente a una empresa cliente y gestionar su facturación, como la razón social, el identificador fiscal y la dirección fiscal. |
| Tramo de facturación anual | Intervalo de facturación anual utilizado para determinar la tarifa aplicable a una empresa cliente. |
| Tarifa | Precio acordado para un servicio o plan contratado por una empresa cliente. |
| Serie de facturación | Conjunto de facturas identificadas mediante una serie y una numeración propia. |
| Factura rectificativa | Factura utilizada para corregir otra factura emitida cuando corresponda. |
| Baja lógica | Desactivación de un registro sin eliminar físicamente su información histórica y sus relaciones asociadas. |
| Aprobación económica | Autorización necesaria para aplicar determinados cambios económicos, como descuentos o modificaciones de tarifas. |
| Prorrateo | Cálculo proporcional del importe correspondiente a una parte de un periodo de facturación. |
