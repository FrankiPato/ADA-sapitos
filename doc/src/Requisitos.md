# Requisitos 

## Requisitos de negocio 
Descripción de las necesidades y objetivos de negocio que motivan el desarrollo del sistema. 

- El sistema deberá permitir gestionar los clientes y la información necesaria para su facturación.
- El sistema deberá permitir generar y gestionar facturas puntuales y recurrentes.
- Las facturas recurrentes deberán generarse mensualmente el día 1 de cada mes.
- Cuando un cliente se incorpore durante el transcurso de un mes, se deberá facturar proporcionalmente el periodo restante hasta finalizar dicho mes y comenzar el ciclo normal el día 1 del mes siguiente.
- El sistema deberá realizar un seguimiento de los pagos y del estado de las facturas de cada cliente.
- El sistema deberá controlar el plazo establecido para el pago y avisar de los retrasos.
- El sistema deberá integrarse con Stripe para validar los pagos realizados.
- Un cliente no podrá darse de baja mientras tenga facturas pendientes de pago o saldo adeudado.
- El sistema deberá proporcionar información e informes sobre la facturación y los pagos para facilitar la gestión administrativa.
- El sistema deberá poder utilizarse desde ordenador, tablet y dispositivos móviles.
- El sistema deberá ser sencillo e intuitivo, centrándose en las funcionalidades necesarias para la gestión de clientes, facturas y pagos.

## Requisitos de usuario 
Descripción de las necesidades y servicios que el sistema debe proporcionar a sus usuarios. 

- Los usuarios deberán poder iniciar sesión mediante sus credenciales
- Los usuarios deberán tener diferentes permisos dependiendo de su rol dentro del sistema
- El administrador deberá poder registrar nuevos clientes introduciendo sus datos fiscales y de contacto
- Los usuarios autorizados deberán poder consultar la información de los clientes y sus facturas
- Los usuarios autorizados deberán poder buscar y filtrar facturas por cliente, fecha y estado
- Los usuarios autorizados deberán poder consultar el estado de los pagos
- Los clientes deberán recibir avisos relacionados con el estado y vencimiento de sus facturas
- Los usuarios con permisos suficientes deberán poder consultar informes sobre la facturación y los pagos
- El administrador deberá disponer de un cuadro de mando con información sobre el estado de la facturación
- El usuario deberá poder utilizar la aplicación tanto desde ordenador como desde dispositivos móviles

## Requisitos del sistema 

### Requisitos Funcionales 
- FR1. El sistema deberá permitir gestionar las cuentas de los usuarios
    - FR1.1. El sistema deberá permitir crear cuentas de usuario
    - FR1.2. El sistema deberá permitir a los usuarios iniciar sesión mediante usuario y contraseña
    - FR1.3. El sistema deberá almacenar las contraseñas de forma segura
    - FR1.4. El sistema deberá negar la entrada a todo aquel con usuario o contraseña incorrectos
    - FR1.5. El sistema deberá controlar el acceso a las funcionalidades según el rol del usuario
    - FR1.6. Los usuarios internos deberán utilizar cuentas asociadas al dominio corporativo establecido por la empresa
- FR2. El sistema deberá gestionar diferentes roles y permisos
    - FR2.1. El sistema deberá contemplar los roles Superadmin, Admin, Lector y Cliente
    - FR2.2. El Superadmin tendrá los mayores privilegios de gestión del sistema
    - FR2.3. El Admin tendrá acceso a las funciones administrativas que le correspondan, sin acceso a las operaciones reservadas al Superadmin
    - FR2.4. El Lector tendrá acceso restringido principalmente a la consulta de información y a modificaciones que no impliquen operaciones críticas
    - FR2.5. El Cliente solo podrá acceder a la información y funcionalidades relacionadas con su propia empresa y sus facturas
    - FR2.6. El sistema deberá impedir que un usuario acceda a funcionalidades para las que no tenga permisos
- FR3. El sistema deberá permitir gestionar los clientes
    - FR3.1. El sistema deberá permitir dar de alta nuevos clientes
    - FR3.2. El sistema deberá permitir introducir el NIF de la empresa
    - FR3.3. El sistema deberá permitir introducir el nombre de la empresa
    - FR3.4. El sistema deberá permitir introducir el correo electrónico del cliente
    - FR3.5. El sistema deberá permitir consultar la información de los clientes
    - FR3.6. El sistema deberá permitir modificar la información de un cliente cuando el usuario disponga de permisos suficientes
    - FR3.7. El sistema deberá permitir dar de baja a un cliente cuando no existan facturas pendientes de pago o saldo adeudado
    - FR3.8. El sistema podrá permitir la importación masiva de clientes mediante archivos CSV
- FR4. El sistema deberá permitir crear y gestionar facturas
    - FR4.1. El sistema deberá permitir generar facturas puntuales
    - FR4.2. El sistema deberá permitir generar facturas recurrentes
    - FR4.3. El sistema deberá generar las facturas recurrentes correspondientes al ciclo mensual el día 1 de cada mes
    - FR4.4. El sistema deberá calcular automáticamente el importe proporcional cuando un cliente se incorpore durante el transcurso de un mes
    - FR4.5. El sistema deberá permitir consultar las facturas asociadas a cada cliente
    - FR4.6. El sistema deberá permitir buscar y filtrar facturas por cliente, intervalo de fechas y estado
    - FR4.7. El sistema deberá permitir modificar una factura pendiente de pago únicamente a los usuarios con permisos suficientes
    - FR4.8. El sistema no deberá permitir modificar una factura que haya sido abonada
- FR5. El sistema deberá controlar el estado de las facturas
    - FR5.1. El sistema deberá registrar si una factura está pendiente de pago
    - FR5.2. El sistema deberá registrar la confirmación de un pago
    - FR5.3. El sistema deberá detectar las facturas cuyo plazo de pago haya expirado sin haberse confirmado el pago
    - FR5.4. El sistema deberá cambiar el estado de la factura cuando se considere impagada
- FR6. El sistema deberá permitir gestionar y validar los pagos de los clientes
    - FR6.1. El sistema deberá integrarse con Stripe
    - FR6.2. El sistema deberá poder iniciar el proceso de cobro mediante Stripe
    - FR6.3. El sistema deberá recibir y registrar la confirmación del pago proporcionada por Stripe
    - FR6.4. El sistema deberá asociar cada pago con la factura correspondiente
    - FR6.5. El sistema deberá actualizar el estado de la factura cuando el pago haya sido validado correctamente
- FR7. El sistema deberá permitir realizar un seguimiento de los pagos y enviar notificaciones
    - FR7.1. El sistema deberá enviar recordatorios de pago mientras la factura permanezca pendiente
    - FR7.2. El sistema deberá enviar los recordatorios de forma periódica durante el plazo establecido
    - FR7.3. El sistema deberá controlar el plazo de pago establecido en 5 días desde la emisión de la factura
    - FR7.4. El sistema deberá identificar como impagada una factura que supere dicho plazo sin confirmación del pago
    - FR7.5. El sistema deberá notificar al Administrador cuando una factura pase a estado de impago
    - FR7.6. El medio utilizado para las notificaciones deberá definirse durante el desarrollo del sistema
- FR8. El sistema deberá permitir generar informes y mostrar un cuadro de mando
    - FR8.1. El sistema deberá proporcionar un cuadro de mando destinado a los usuarios administrativos autorizados
    - FR8.2. El cuadro de mando deberá mostrar información relevante sobre facturación y pagos
    - FR8.3. El sistema deberá permitir generar informes sobre la actividad de facturación
    - FR8.4. El sistema deberá permitir generar informes con periodicidad mensual y anual
    - FR8.5. El acceso al cuadro de mando y a los informes deberá estar restringido según los permisos del usuario

### Requisitos No-Funcionales  
- NFR1. Seguridad de los datos. Toda la información sensible, incluyendo credenciales, información fiscal, facturación y pagos, deberá almacenarse y transmitirse utilizando mecanismos de seguridad adecuados.
- NFR2. Control de acceso. El sistema deberá utilizar un mecanismo de control de acceso basado en roles para limitar las funcionalidades disponibles para cada tipo de usuario.
- NFR3. Protección de contraseñas. Las contraseñas no deberán almacenarse en texto plano y deberán utilizar mecanismos seguros para su almacenamiento.
- NFR4. Responsive / Multiplataforma. El sistema deberá visualizarse correctamente tanto en móvil, tablet como en ordenador.
- NFR5. Usabilidad. La interfaz deberá ser sencilla, intuitiva y fácil de utilizar, priorizando las funcionalidades principales de gestión de clientes, facturas y pagos.
- NFR6. Escalabilidad de datos. El sistema deberá poder almacenar información de múltiples clientes, facturas y pagos sin que el crecimiento normal de los datos impida el funcionamiento de las funcionalidades principales.
- NFR7. Concurrencia. El sistema deberá soportar el acceso simultáneo de varios usuarios sin provocar inconsistencias en la información almacenada.
- NFR8. Rendimiento. Las operaciones habituales de consulta, búsqueda y navegación deberán ejecutarse de forma fluida. Las operaciones que requieran un procesamiento elevado, como la generación de informes o facturas masivas, no deberán bloquear las funciones básicas del sistema.
- NFR9. Disponibilidad. El sistema deberá estar disponible durante el horario de funcionamiento establecido por la empresa, salvo durante tareas programadas de mantenimiento.
- NFR10. Despliegue. El sistema deberá permitir su instalación y ejecución en un entorno de servidor local (on-premises).
- NFR11. Integridad de los datos. El sistema deberá mantener la coherencia entre clientes, facturas y pagos, evitando que una operación pueda producir registros inconsistentes.
- NFR12. Mantenibilidad. El sistema deberá estar estructurado de forma que sus componentes puedan mantenerse, corregirse y ampliarse sin afectar innecesariamente al resto de funcionalidades.
- NFR13. Retención de información. Los datos relacionados con clientes, facturación y pagos deberán conservarse durante el periodo que corresponda conforme a las obligaciones legales y fiscales aplicables a la empresa.
