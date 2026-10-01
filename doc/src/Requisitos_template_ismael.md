# Requisitos 

## Requisitos de negocio 
Descripción de las necesidades y objetivos de negocio que motivan el desarrollo del sistema.

La necesidad del negocio se basa en crear una aplicación web que pueda funcionar en cualquier dispositivo con el fin de generar facturas a los clientes que contratan el servicio de Turbineh.
El objetivo es poder tener un seguimiento claro de los pagos futuros y pasados para controlar a todos los clientes.

## Requisitos de usuario 
Descripción de las necesidades y servicios que el sistema debe proporcionar a sus usuarios.

- El usuario debe poder acceder con su correo y contraseña
- El usuario podrá descargar información propia disponible
- El usuario podrá consultar sus facturas proximas y pasadas
- El usuario podrá recibir notificaciones en un método de contacto habilitado por él (email o whatsapp)
- El usuario tendrá opciones de filtrado para facilitar la busqueda de empresas
- El usuario podrá cambiar o recuperar la contraseña mediante un método de recuperación
- El usuario podrá darse de baja si cumple las condiciones para ello

Dependiendo de los diferentes roles (lector, administrador y super-administrador): 
- El lector podrá ver y descargar información disponible para él
- El lector tendrá acceso restringido a modificaciones y a cierta información
- El administrador puede gestionar lo que ve el cliente
- El super-administrador tendrá acceso a opciones de gestión más profundas

## Requisitos del sistema 

### Requisitos Funcionales 
- FR1. El sistema debe permitir acceder al usuario a la aplicacion con email y contraseña
    -FR1.1 El sistema deberá de verificar que el email utilizado corresponde a @turbineh
    -FR1.2 En caso de querer recuperar contraseña, el sistema tendrá una opción habilitada para ello
- FR2. El sistema debe enviar notificaciones al usuario mediante un método de contacto
- FR3. El sistema debe tener una opción de busqueda y de filtrado
- FR4. El sistema debe guardar la información personal de todos los clientes registrados
    -FR4.1 En caso de que un cliente se dé de baja en la aplicación, el sistema podrá guardar la información lo máximo permitido por la ley
- FR5. El sistema debe guardar toda la información de facturas ya realizadas
- FR6. El sistema debe permitir la importación de clientes a través de un CSV
- FR7. El sistema podrá permitir al usuario darse de baja
    FR7.1 Solo podrá darse de baja si no tiene ninguna factura pendiente de pago, en caso de que la tenga no es posible


### Requisitos No-Funcionales 
- NFR1. El sistema debe poder usarse en cualquier dispositivo y adaptarse a él
- NFR2. El sistema debe de ser lo más eficiente posible 
    - NFR2.1 Debe de funcionar en pocos clicks y obtener respuesta rápida en el menor tiempo posible
- NFR3. El sistema debe de ser escalable independientemente del número de usuarios conectados
- NFR4. El sistema debe de estar disponible siempre, las 24 horas del día
- NFR5. El sistema tendrá un control de acceso basado en los diferentes roles
