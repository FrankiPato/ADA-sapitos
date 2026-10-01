# Requisitos 

## Requisitos de negocio 
Descripción de las necesidades y objetivos de negocio que motivan el desarrollo del sistema.


## Requisitos de usuario 
Descripción de las necesidades y servicios que el sistema debe proporcionar a sus usuarios. 

## Requisitos del sistema 

### Requisitos Funcionales 
- FR1. El sistema deberá permitir al administrador crear cuentas de usuarios

- FR2. El sistema deberá permitir a los usuarios iniciar sesión con sus credenciales
    - FR2.1 El sistema deberá almacenar el usuario, y la contraseña cifrada, en el sistema
    - FR2.2 El sistema deberá negar la entrada a todo aquel con usuario o contraseña incorrectas

- FR3. El sistema deberá guardar información sobre los pagos y facturas de los clientes
    - FR3.1 El sistema deberá generar facturas mensuales
    - FR3.2 El sistema deberá generar facturas trimestrales
    - FR3.3 El sistema deberá generar facturas anuales
  
- FR4. El sistema deberá detectar impagos y notificarlos al cliente por algún medio de comunicación (por definir el medio de comunicación)
  
- FR5. El sistema deberá permitir dar de alta a nuevos clientes
    - FR5.1 El sistema deberá permitir introducir Nombre de la empresa
  
- FR6. El sistema deberá permitir dar de baja a viejos clientes

- FR7. El sistema deberá comprobar en que umbral de pago se encuentra cada cliente al final del año fiscal o al ser añadido como nuevo cliente
    -FR7.1 El sistema deberá actualizar el umbral de pago de cada cliente si aplica
  
- FR8. El sistema deberá permitir mostrar los usuarios 
 

### Requisitos No-Funcionales  
- NFR1. Escalabilidad de datos. El sistema deberá poder almacenar información de múltiples clientes de manera simultánea sin degradación de servicio.
- NFR2. Concurrencia.  El sistema deberá soportar el uso simultáneo de múltiples usuarios sin afectar los tiempos de respuesta
- NFR3. Seguridad de los datos. Toda la información sensible, especialmente los datos de facturación y contraseñas, deberá ser almacenada y transmitida utilizando protocolos de cifrado seguro.
- NFR4. Disponibilidad. El sistema deberá garantizar una disponibilidad del 99.9% durante el horario laboral estipulado
- NFR5. Rendimiento. El proceso de generación de facturas masivas o la comprobación de umbrales no deberá bloquear funciones básicas como inicio de sesión o consulta de usuarios.
- NFR6. Multiplataforma. El sistema deberá visualizarse correctamente tanto en móvil, tablet como en ordenador
-  
- ...
