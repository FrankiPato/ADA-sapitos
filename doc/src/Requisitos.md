Requisitos
Requisitos de negocio

Descripción de las necesidades y objetivos de negocio que motivan el desarrollo del sistema.

BR1. Gestión de clientes. El sistema deberá permitir gestionar las empresas clientes, incluyendo su alta, modificación y baja, manteniendo la información necesaria para la gestión de la facturación.

BR2. Gestión de facturación. El sistema deberá permitir generar y gestionar facturas correspondientes a los servicios prestados a los clientes, incluyendo facturas puntuales y recurrentes.

BR3. Facturación recurrente. Las facturas recurrentes deberán generarse siguiendo un ciclo mensual, consolidándose y emitiéndose el día 1 de cada mes.

BR4. Prorrateo de nuevas altas. Cuando un cliente inicie el servicio en una fecha distinta al día 1 del mes, se deberá facturar proporcionalmente el periodo restante hasta finalizar dicho mes. A partir del mes siguiente, el cliente entrará en el ciclo ordinario de facturación del día 1.

BR5. Seguimiento de pagos. El sistema deberá realizar un seguimiento del estado de las facturas y de los pagos asociados a cada cliente.

BR6. Gestión de retrasos. El sistema deberá controlar el plazo establecido para el pago de las facturas y generar los avisos correspondientes cuando una factura no haya sido abonada dentro del plazo establecido.

BR7. Integración con Stripe. El sistema deberá integrarse con Stripe para gestionar y validar los pagos realizados por los clientes.

BR8. Gestión de bajas. Un cliente no podrá darse de baja mientras mantenga facturas pendientes de pago o saldo adeudado.

BR9. Informes y supervisión. El sistema deberá proporcionar información consolidada sobre la facturación y los pagos para facilitar la supervisión y gestión administrativa.

BR10. Accesibilidad desde diferentes dispositivos. La aplicación deberá poder utilizarse desde ordenador, tablet y dispositivos móviles, adaptando su interfaz a cada tipo de dispositivo.

BR11. Simplicidad de uso. La aplicación deberá centrarse en las funciones necesarias para la gestión de clientes, facturas y pagos, proporcionando una interfaz sencilla e intuitiva.

Requisitos de usuario

Descripción de las necesidades y servicios que el sistema debe proporcionar a sus usuarios.

UR1. Los usuarios deberán poder iniciar sesión mediante sus credenciales de acceso.

UR2. Los usuarios deberán disponer de diferentes permisos en función de su rol dentro del sistema.

UR3. El administrador deberá poder registrar nuevos clientes introduciendo sus datos fiscales y de contacto.

UR4. El administrador podrá disponer de un mecanismo de importación de clientes mediante archivos CSV para facilitar el registro de múltiples empresas.

UR5. Los usuarios autorizados deberán poder consultar la información de los clientes y sus facturas.

UR6. Los usuarios autorizados deberán poder buscar y filtrar facturas por diferentes criterios, como cliente, fecha y estado.

UR7. Los usuarios autorizados deberán poder consultar el estado de los pagos de las facturas.

UR8. Los clientes deberán recibir avisos relacionados con el estado y vencimiento de sus facturas.

UR9. Los usuarios con permisos suficientes deberán poder consultar informes sobre la facturación y los pagos.

UR10. El administrador deberá disponer de un cuadro de mando con información resumida sobre el estado de la facturación.

UR11. El usuario deberá poder utilizar la aplicación tanto desde ordenador como desde dispositivos móviles.

Requisitos del sistema
Requisitos Funcionales

FR1. Gestión de usuarios y autenticación. El sistema deberá permitir gestionar las cuentas de los usuarios internos.

FR1.1. El sistema deberá permitir crear cuentas de usuario.

FR1.2. El sistema deberá permitir a los usuarios iniciar sesión mediante usuario y contraseña.

FR1.3. El sistema deberá almacenar las contraseñas de forma segura.

FR1.4. El sistema deberá impedir el acceso cuando las credenciales proporcionadas sean incorrectas.

FR1.5. El sistema deberá controlar el acceso a las funcionalidades según el rol del usuario.

FR1.6. Los usuarios internos deberán utilizar cuentas asociadas al dominio corporativo establecido por la empresa.

FR2. Gestión de roles y permisos. El sistema deberá gestionar diferentes niveles de acceso.

FR2.1. El sistema deberá contemplar los roles Superadmin, Admin, Lector y Cliente.

FR2.2. El Superadmin tendrá los mayores privilegios de gestión del sistema.

FR2.3. El Admin tendrá acceso a las funciones administrativas que le sean asignadas, sin acceso a las operaciones reservadas al Superadmin.

FR2.4. El Lector tendrá acceso restringido principalmente a la consulta de información y a aquellas modificaciones que no impliquen operaciones críticas.

FR2.5. El Cliente solo podrá acceder a la información y funcionalidades relacionadas con su propia empresa y sus facturas.

FR2.6. El sistema deberá impedir que un usuario acceda a funcionalidades para las que no tenga permisos.

FR3. Gestión de clientes. El sistema deberá permitir gestionar las empresas clientes.

FR3.1. El sistema deberá permitir dar de alta nuevos clientes.

FR3.2. El sistema deberá permitir introducir como mínimo el NIF de la empresa, nombre de la empresa y correo electrónico.

FR3.3. El sistema deberá permitir consultar la información de los clientes.

FR3.4. El sistema deberá permitir modificar la información de un cliente cuando el usuario disponga de permisos suficientes.

FR3.5. El sistema deberá permitir procesar la baja de un cliente cuando no existan facturas pendientes de pago o saldo adeudado.

FR3.6. El sistema podrá permitir la importación masiva de clientes mediante archivos CSV.

FR4. Gestión de facturas. El sistema deberá permitir crear y gestionar las facturas de los clientes.

FR4.1. El sistema deberá permitir generar facturas puntuales.

FR4.2. El sistema deberá permitir generar facturas recurrentes.

FR4.3. El sistema deberá generar las facturas recurrentes correspondientes al ciclo mensual el día 1 de cada mes.

FR4.4. El sistema deberá calcular automáticamente el importe proporcional correspondiente cuando un cliente se incorpore durante el transcurso de un mes.

FR4.5. El sistema deberá permitir consultar las facturas asociadas a cada cliente.

FR4.6. El sistema deberá permitir buscar y filtrar facturas por cliente, intervalo de fechas y estado.

FR4.7. El sistema deberá permitir modificar una factura pendiente de pago únicamente a los usuarios con permisos suficientes.

FR4.8. Una factura que haya sido abonada no podrá ser modificada.

FR5. Gestión del estado de las facturas. El sistema deberá controlar el estado de cada factura.

FR5.1. El sistema deberá registrar si una factura está pendiente de pago.

FR5.2. El sistema deberá registrar la confirmación de un pago.

FR5.3. El sistema deberá identificar las facturas cuyo plazo de pago haya expirado sin haberse confirmado el pago.

FR5.4. Las facturas que superen el plazo establecido sin pago confirmado deberán pasar al estado correspondiente de impago.

FR6. Gestión de pagos. El sistema deberá permitir gestionar y validar los pagos realizados por los clientes.

FR6.1. El sistema deberá integrarse con Stripe.

FR6.2. El sistema deberá poder iniciar el proceso de cobro mediante Stripe.

FR6.3. El sistema deberá recibir y registrar la confirmación del pago proporcionada por Stripe.

FR6.4. El sistema deberá asociar cada pago con la factura correspondiente.

FR6.5. El sistema deberá actualizar el estado de la factura cuando el pago haya sido validado correctamente.

FR7. Notificaciones y seguimiento de pagos. El sistema deberá avisar a los clientes sobre el estado de sus pagos.

FR7.1. El sistema deberá enviar recordatorios de pago durante el plazo establecido.

FR7.2. Los recordatorios deberán enviarse periódicamente mientras la factura permanezca pendiente.

FR7.3. El sistema deberá controlar el plazo de pago establecido en 5 días desde la emisión de la factura.

FR7.4. Una vez superado dicho plazo sin confirmación del pago, el sistema deberá identificar la factura como impagada.

FR7.5. El sistema deberá notificar al Administrador cuando una factura pase a estado de impago.

FR7.6. El medio concreto utilizado para las notificaciones deberá definirse durante el desarrollo del sistema.

FR8. Informes y cuadro de mando. El sistema deberá proporcionar información sobre la actividad de facturación.

FR8.1. El sistema deberá proporcionar un cuadro de mando destinado a los usuarios administrativos autorizados.

FR8.2. El cuadro de mando deberá mostrar información relevante sobre facturación y pagos.

FR8.3. El sistema deberá permitir generar informes consolidados de la actividad de facturación.

FR8.4. Los informes deberán poder generarse, como mínimo, con periodicidad mensual y anual.

FR8.5. El acceso al cuadro de mando y a los informes deberá estar restringido según los permisos del usuario.

Requisitos No-Funcionales

NFR1. Seguridad de los datos. La información sensible, incluyendo credenciales, información fiscal y datos relacionados con facturación y pagos, deberá almacenarse y transmitirse utilizando mecanismos de seguridad adecuados.

NFR2. Control de acceso. El sistema deberá aplicar un mecanismo de control de acceso basado en roles (RBAC) para limitar las funcionalidades disponibles para cada tipo de usuario.

NFR3. Protección de contraseñas. Las contraseñas no deberán almacenarse en texto plano y deberán utilizar mecanismos de cifrado o hash adecuados para su almacenamiento seguro.

NFR4. Responsive / Multiplataforma. La interfaz deberá visualizarse y funcionar correctamente en ordenadores, tablets y dispositivos móviles.

NFR5. Usabilidad. La interfaz deberá ser sencilla, intuitiva y coherente, priorizando las funcionalidades principales de gestión de clientes, facturas y pagos.

NFR6. Escalabilidad de datos. El sistema deberá ser capaz de almacenar y gestionar información correspondiente a múltiples clientes, facturas y pagos sin que el crecimiento normal de los datos impida el funcionamiento de las funcionalidades principales.

NFR7. Concurrencia. El sistema deberá permitir el acceso simultáneo de varios usuarios sin provocar inconsistencias en la información almacenada.

NFR8. Rendimiento. Las operaciones habituales de consulta, búsqueda y navegación deberán ejecutarse en un tiempo adecuado para permitir un uso fluido de la aplicación. Las operaciones que puedan requerir un procesamiento elevado, como la generación de informes o facturas masivas, no deberán impedir el funcionamiento de las funcionalidades básicas del sistema.

NFR9. Disponibilidad. El sistema deberá estar disponible durante el horario de funcionamiento establecido por la empresa, salvo durante tareas programadas de mantenimiento.

NFR10. Despliegue. El sistema deberá poder instalarse y ejecutarse en un entorno de servidor local (on-premises).

NFR11. Integridad de los datos. El sistema deberá mantener la coherencia entre clientes, facturas y pagos, evitando que una operación pueda producir registros inconsistentes.

NFR12. Mantenibilidad. El sistema deberá estar estructurado de forma que sus componentes puedan mantenerse, corregirse y ampliarse sin afectar innecesariamente al resto de funcionalidades.

NFR13. Retención de información. Los datos relacionados con clientes, facturación y pagos deberán conservarse durante el periodo que corresponda conforme a las obligaciones legales y fiscales aplicables a la empresa.
