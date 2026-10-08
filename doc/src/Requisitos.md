# Requisitos

## 1. Contexto y alcance

### 1.1. Objetivo del sistema

El sistema tiene como objetivo proporcionar una aplicación para gestionar los clientes de la empresa y la información relacionada con su facturación y sus pagos.

El sistema permitirá gestionar clientes, generar y consultar facturas puntuales y recurrentes, realizar el seguimiento de los pagos, controlar los retrasos y proporcionar información e informes que faciliten la gestión administrativa.

Además, el sistema deberá integrarse con Stripe para validar los pagos realizados por los clientes y deberá proporcionar diferentes niveles de acceso en función del rol de cada usuario.

### 1.2. Alcance

El sistema será responsable de:

- Gestionar las cuentas de los usuarios y sus permisos.
- Gestionar diferentes roles de usuario.
- Gestionar la información de los clientes.
- Generar y gestionar facturas puntuales y recurrentes.
- Calcular la facturación proporcional de los clientes que se incorporen durante un mes.
- Controlar el estado de las facturas y de los pagos.
- Integrarse con Stripe para iniciar y validar los pagos.
- Enviar recordatorios y notificaciones relacionados con los pagos.
- Generar informes sobre facturación y pagos.
- Proporcionar un cuadro de mando para usuarios administrativos autorizados.
- Permitir el acceso desde ordenador, tablet y dispositivos móviles.
- Garantizar la seguridad, integridad y conservación de la información.

El documento original no especifica funcionalidades ajenas a la gestión de clientes, facturación, pagos, usuarios e información administrativa, por lo que dichas funcionalidades quedan fuera del alcance definido.

### 1.3. Actores y partes interesadas

| Actor / stakeholder | Descripción | Intereses principales |
|---|---|---|
| Superadmin | Usuario con los mayores privilegios de gestión del sistema | Gestionar el sistema y disponer de los mayores privilegios de administración |
| Admin | Usuario con funciones administrativas | Gestionar clientes, facturas, pagos e información administrativa según sus permisos |
| Lector | Usuario con acceso principalmente a la consulta de información | Consultar información sin realizar operaciones críticas |
| Cliente | Empresa cliente que utiliza el sistema para consultar información relacionada con su propia empresa y sus facturas | Consultar sus facturas y recibir información relacionada con pagos y vencimientos |
| Empresa | Organización que utiliza el sistema para gestionar clientes, facturación y pagos | Facilitar y mejorar la gestión administrativa y el seguimiento de los cobros |
| Stripe | Sistema externo utilizado para gestionar y validar pagos | Proporcionar confirmación de los pagos realizados |

## 2. Requisitos de negocio

Los siguientes requisitos representan las necesidades y objetivos de negocio identificados en el documento original.

| ID | Requisito | Fuente |
|---|---|---|
| BR-01 | Gestionar los clientes y la información necesaria para su facturación | Documento original |
| BR-02 | Gestionar facturas puntuales y recurrentes | Documento original |
| BR-03 | Generar mensualmente las facturas recurrentes correspondientes | Documento original |
| BR-04 | Aplicar una facturación proporcional a los clientes que se incorporen durante un mes | Documento original |
| BR-05 | Realizar un seguimiento de los pagos y del estado de las facturas de cada cliente | Documento original |
| BR-06 | Controlar los plazos de pago y detectar los retrasos | Documento original |
| BR-07 | Integrarse con Stripe para validar los pagos realizados | Documento original |
| BR-08 | Impedir la baja de un cliente cuando tenga facturas pendientes de pago o saldo adeudado | Documento original |
| BR-09 | Proporcionar información e informes sobre facturación y pagos para facilitar la gestión administrativa | Documento original |
| BR-10 | Permitir el uso del sistema desde ordenador, tablet y dispositivos móviles | Documento original |
| BR-11 | Proporcionar una interfaz sencilla e intuitiva centrada en la gestión de clientes, facturas y pagos | Documento original |

## 3. Requisitos de usuario

Los requisitos de usuario se han separado de los requisitos funcionales, expresándolos desde el punto de vista de las necesidades de los distintos usuarios.

| ID | Actor | Requisito | Fuente |
|---|---|---|---|
| UR-01 | Usuarios | Poder iniciar sesión mediante sus credenciales | Documento original |
| UR-02 | Usuarios | Disponer de permisos diferentes según el rol que tengan dentro del sistema | Documento original |
| UR-03 | Administrador | Poder registrar nuevos clientes introduciendo sus datos fiscales y de contacto | Documento original |
| UR-04 | Usuarios autorizados | Poder consultar la información de los clientes y sus facturas | Documento original |
| UR-05 | Usuarios autorizados | Poder buscar y filtrar facturas por cliente, fecha y estado | Documento original |
| UR-06 | Usuarios autorizados | Poder consultar el estado de los pagos | Documento original |
| UR-07 | Clientes | Recibir avisos relacionados con el estado y vencimiento de sus facturas | Documento original |
| UR-08 | Usuarios con permisos suficientes | Poder consultar informes sobre facturación y pagos | Documento original |
| UR-09 | Administrador | Disponer de un cuadro de mando con información sobre el estado de la facturación | Documento original |
| UR-10 | Usuarios | Poder utilizar la aplicación desde ordenador y dispositivos móviles | Documento original |

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
| FR-11 | El Cliente solo podrá acceder a la información y funcionalidades relacionadas con su propia empresa y sus facturas | UR-02 |
| FR-12 | El sistema deberá impedir que un usuario acceda a funcionalidades para las que no tenga permisos | UR-02 |
| FR-13 | El sistema deberá permitir dar de alta nuevos clientes | UR-03 |
| FR-14 | El sistema deberá permitir introducir el NIF de la empresa | UR-03 |
| FR-15 | El sistema deberá permitir introducir el nombre de la empresa | UR-03 |
| FR-16 | El sistema deberá permitir introducir el correo electrónico del cliente | UR-03 |
| FR-17 | El sistema deberá permitir consultar la información de los clientes | UR-04 |
| FR-18 | El sistema deberá permitir modificar la información de un cliente cuando el usuario disponga de permisos suficientes | UR-03, UR-04 |
| FR-19 | El sistema deberá permitir dar de baja a un cliente cuando no existan facturas pendientes de pago o saldo adeudado | UR-03 |
| FR-20 | El sistema podrá permitir la importación masiva de clientes mediante archivos CSV | UR-03 |
| FR-21 | El sistema deberá permitir generar facturas puntuales | UR-04 |
| FR-22 | El sistema deberá permitir generar facturas recurrentes | UR-04 |
| FR-23 | El sistema deberá generar las facturas recurrentes correspondientes al ciclo mensual el día 1 de cada mes | UR-04 |
| FR-24 | El sistema deberá calcular automáticamente el importe proporcional cuando un cliente se incorpore durante el transcurso de un mes | UR-04 |
| FR-25 | El sistema deberá permitir consultar las facturas asociadas a cada cliente | UR-04 |
| FR-26 | El sistema deberá permitir buscar y filtrar facturas por cliente, intervalo de fechas y estado | UR-05 |
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

### 4.2. Requisitos no funcionales

Los requisitos no funcionales se han reformulado para incluir, cuando es posible, un criterio que permita verificar su cumplimiento. El documento original define las categorías de seguridad, control de acceso, protección de contraseñas, multiplataforma, usabilidad, escalabilidad, concurrencia, rendimiento, disponibilidad, despliegue, integridad, mantenibilidad y retención.

| ID | Categoría | Requisito | Criterio verificable |
|---|---|---|---|
| NFR-01 | Seguridad | La información sensible, incluyendo credenciales, información fiscal, facturación y pagos, deberá almacenarse y transmitirse utilizando mecanismos de seguridad adecuados | Las credenciales y datos sensibles no deberán almacenarse ni transmitirse sin los mecanismos de protección definidos para el sistema |
| NFR-02 | Control de acceso | El sistema deberá utilizar un mecanismo de control de acceso basado en roles | Un usuario no deberá poder acceder a funcionalidades que no correspondan a su rol |
| NFR-03 | Seguridad | Las contraseñas no deberán almacenarse en texto plano | La información almacenada correspondiente a las contraseñas no deberá permitir obtener directamente la contraseña original |
| NFR-04 | Compatibilidad / Responsive | El sistema deberá visualizarse correctamente en móvil, tablet y ordenador | Las principales funcionalidades deberán poder utilizarse desde los tres tipos de dispositivo |
| NFR-05 | Usabilidad | La interfaz deberá ser sencilla, intuitiva y estar centrada en las funcionalidades principales | Las funcionalidades principales de clientes, facturas y pagos deberán poder utilizarse sin conocimientos técnicos específicos |
| NFR-06 | Escalabilidad | El sistema deberá poder almacenar información de múltiples clientes, facturas y pagos sin que el crecimiento normal de los datos impida el funcionamiento de las funcionalidades principales | El crecimiento de clientes, facturas y pagos no deberá impedir el funcionamiento de las funcionalidades principales |
| NFR-07 | Concurrencia | El sistema deberá soportar el acceso simultáneo de varios usuarios sin provocar inconsistencias en la información almacenada | Las operaciones simultáneas no deberán generar datos inconsistentes |
| NFR-08 | Rendimiento | Las operaciones habituales de consulta, búsqueda y navegación deberán ejecutarse de forma fluida | Las operaciones habituales deberán poder ejecutarse sin bloquear las funciones básicas del sistema |
| NFR-09 | Rendimiento | Las operaciones que requieran un procesamiento elevado, como la generación de informes o facturas masivas, no deberán bloquear las funciones básicas del sistema | La generación de informes o facturas masivas no deberá impedir el uso de las funciones básicas |
| NFR-10 | Disponibilidad | El sistema deberá estar disponible durante el horario de funcionamiento establecido por la empresa, salvo durante tareas programadas de mantenimiento | El sistema deberá estar disponible durante el horario establecido excepto durante los mantenimientos programados |
| NFR-11 | Despliegue | El sistema deberá permitir su instalación y ejecución en un entorno de servidor local (on-premises) | La aplicación deberá poder instalarse y ejecutarse en un servidor local |
| NFR-12 | Integridad | El sistema deberá mantener la coherencia entre clientes, facturas y pagos | Las operaciones no deberán producir registros inconsistentes entre clientes, facturas y pagos |
| NFR-13 | Mantenibilidad | El sistema deberá estar estructurado de forma que sus componentes puedan mantenerse, corregirse y ampliarse sin afectar innecesariamente al resto de funcionalidades | Los cambios o correcciones de un componente no deberán requerir modificaciones innecesarias en el resto del sistema |
| NFR-14 | Retención | Los datos relacionados con clientes, facturación y pagos deberán conservarse durante el periodo que corresponda conforme a las obligaciones legales y fiscales aplicables | La información deberá conservarse durante el periodo legal o fiscal que corresponda |

## 5. Reglas de negocio

Estas reglas representan condiciones del dominio que condicionan el comportamiento del sistema.

| ID | Regla |
|---|---|
| RB-01 | Las facturas recurrentes deberán generarse mensualmente el día 1 de cada mes. |
| RB-02 | Cuando un cliente se incorpore durante el transcurso de un mes, deberá facturarse proporcionalmente el periodo restante hasta finalizar dicho mes. |
| RB-03 | A partir del mes siguiente a la incorporación, el cliente comenzará el ciclo normal de facturación el día 1. |
| RB-04 | Un cliente no podrá darse de baja mientras tenga facturas pendientes de pago o saldo adeudado. |
| RB-05 | El plazo de pago establecido será de 5 días desde la emisión de la factura. |
| RB-06 | Una factura que supere el plazo establecido sin confirmación del pago deberá identificarse como impagada. |
| RB-07 | Una factura que haya sido abonada no podrá modificarse. |
| RB-08 | El acceso a las funcionalidades del sistema dependerá del rol y permisos del usuario. |
| RB-09 | Un Cliente solo podrá acceder a la información y funcionalidades relacionadas con su propia empresa y sus facturas. |

## 6. Suposiciones y decisiones de análisis

Se documentan aquí las decisiones que se pueden identificar en el documento original y que son necesarias para interpretar los requisitos.

| ID | Suposición o decisión | Justificación | Requisitos afectados |
|---|---|---|---|
| A-01 | Se considera que los roles del sistema serán Superadmin, Admin, Lector y Cliente | El documento original especifica expresamente estos cuatro roles | FR-07 a FR-12, NFR-02 |
| A-02 | Se considera que el plazo de pago es de 5 días naturales desde la emisión de la factura, a falta de una especificación más concreta | El documento establece un plazo de 5 días, pero no especifica si se trata de días naturales o laborables | FR-40, FR-41, RB-05, RB-06 |
| A-03 | La importación masiva mediante CSV se considera una funcionalidad opcional | El documento original utiliza explícitamente la expresión "podrá permitir" | FR-20 |
| A-04 | Se considera que la validación realizada mediante Stripe es suficiente para confirmar un pago en el sistema | El documento establece que Stripe proporcionará la confirmación del pago | FR-33 a FR-37 |
| A-05 | Se considera que los informes podrán generarse con periodicidad mensual y anual | El documento original establece expresamente ambas periodicidades | FR-45, FR-46 |

## 7. Dudas, ambigüedades y cuestiones pendientes

| ID | Cuestión | Origen | Impacto | Estado / resolución |
|---|---|---|---|---|
| Q-01 | ¿Cuál será el medio utilizado para enviar las notificaciones y recordatorios de pago? | Documento original | Afecta al mecanismo de notificaciones y a FR-38, FR-39 y FR-42 | Pendiente: el documento indica que deberá definirse durante el desarrollo |
| Q-02 | ¿Los 5 días de plazo de pago son naturales o laborables? | Documento original | Afecta al cálculo del vencimiento y a la detección de impagos | Pendiente |
| Q-03 | ¿Qué se considera exactamente "saldo adeudado" a efectos de impedir la baja de un cliente? | Documento original | Afecta a la condición de baja del cliente | Pendiente |
| Q-04 | ¿Qué información concreta deberá mostrar el cuadro de mando? | Documento original | Afecta al contenido de FR-43 y FR-44 | Pendiente |
| Q-05 | ¿Qué información concreta deberán contener los informes mensuales y anuales? | Documento original | Afecta al contenido de FR-45 y FR-46 | Pendiente |
| Q-06 | ¿Qué operaciones están reservadas exclusivamente al Superadmin? | Documento original | Afecta a la diferenciación de permisos entre Superadmin y Admin | Pendiente |
| Q-07 | ¿Qué modificaciones se consideran operaciones no críticas para el rol Lector? | Documento original | Afecta a los permisos del rol Lector | Pendiente |

## 8. Trazabilidad

La siguiente tabla relaciona los objetivos de negocio con las necesidades de los usuarios y los requisitos del sistema.

| Requisito de negocio | Requisito de usuario | Requisito funcional / no funcional |
|---|---|---|
| BR-01 | UR-03, UR-04 | FR-13 a FR-20, NFR-06, NFR-12 |
| BR-02 | UR-04 | FR-21 a FR-24 |
| BR-03 | UR-04 | FR-23, RB-01 |
| BR-04 | UR-04 | FR-24, RB-02, RB-03 |
| BR-05 | UR-06 | FR-29 a FR-32, FR-36, FR-37 |
| BR-06 | UR-07 | FR-31, FR-32, FR-38 a FR-42, RB-05, RB-06 |
| BR-07 | UR-06 | FR-33 a FR-37 |
| BR-08 | UR-03 | FR-19, RB-04 |
| BR-09 | UR-08, UR-09 | FR-43 a FR-47 |
| BR-10 | UR-10 | NFR-04 |
| BR-11 | UR-04, UR-05, UR-06, UR-08, UR-09 | NFR-05, NFR-08, NFR-09 |
| — | UR-01 | FR-01 a FR-04, NFR-03 |
| — | UR-02 | FR-05 a FR-12, NFR-02 |
| — | UR-05 | FR-26 |
| — | UR-07 | FR-38 a FR-42 |
| — | UR-08 | FR-45 a FR-47 |
| — | UR-09 | FR-43, FR-44, FR-47 |

## 9. Glosario

| Término | Definición |
|---|---|
| Cliente | Empresa que utiliza los servicios de la organización y para la que se gestionan facturas y pagos en el sistema. |
| Factura puntual | Factura generada de manera individual y no asociada a un ciclo recurrente. |
| Factura recurrente | Factura que forma parte de un ciclo periódico de facturación mensual. |
| Facturación proporcional | Importe calculado para cubrir el periodo restante del mes cuando un cliente se incorpora durante el transcurso del mismo. |
| Pago | Operación mediante la cual un cliente abona el importe correspondiente a una factura. |
| Factura pendiente | Factura cuyo pago todavía no ha sido confirmado. |
| Factura impagada | Factura cuyo plazo de pago ha expirado sin que se haya confirmado el pago. |
| Superadmin | Rol de usuario con los mayores privilegios de gestión del sistema. |
| Admin | Rol de usuario con funciones administrativas, pero sin acceso a las operaciones reservadas al Superadmin. |
| Lector | Rol con acceso principalmente a la consulta de información y a modificaciones que no impliquen operaciones críticas. |
| Cliente (rol) | Rol que permite acceder únicamente a la información y funcionalidades relacionadas con la propia empresa y sus facturas. |
| Stripe | Sistema externo con el que se integra la aplicación para iniciar y validar pagos. |
| Cuadro de mando | Interfaz destinada a usuarios administrativos autorizados que muestra información relevante sobre facturación y pagos. |
| CSV | Formato de archivo que podrá utilizarse para la importación masiva de clientes. |
| On-premises | Entorno en el que el sistema se instala y ejecuta en un servidor local de la empresa. |
