# Especificación de casos de uso

En este documento se describen los 27 casos de uso del sistema de facturación y cobros de Turbine, representados en los cinco diagramas de casos de uso (`casos_uso.pu`).

- **Actores principales:** Usuario operativo, Administrador y Superadministrador. El Administrador hereda los casos de uso del Usuario operativo y el Superadministrador los del Administrador (RB-18). Por eso, en cada caso de uso se indica el rol mínimo que puede realizarlo.
- **Actores secundarios:** Planificador del sistema, Stripe (simulado), Servicio de mensajería (simulado) y Sistema externo vía API.
- Los clientes de Turbine y la gestoría no son actores, ya que no utilizan la aplicación.

En los flujos, "Usuario" se refiere al usuario interno que realiza el caso de uso, sea cual sea su rol.



## CU-01 — Iniciar sesión

### 1. Identificación

| Campo | Descripción |
|---|---|
| **Identificador** | CU-01 |
| **Nombre** | Iniciar sesión |
| **Objetivo** | Permitir que un empleado de Turbine acceda a la aplicación con su cuenta corporativa y con los permisos de su rol. |
| **Actores participantes** | Usuario operativo (principal; también Administrador y Superadministrador por herencia) |
| **Relaciones** | Extendido por CU-02 (Recuperar contraseña) |
| **Requisitos relacionados** | UR-01; FR-01, FR-02, FR-04; NFR-15, NFR-16, NFR-17 |

---

### 2. Precondiciones

- PRE-1. El usuario tiene una cuenta creada por un administrador.
- PRE-2. La cuenta está activa.

---

### 3. Flujo principal

1. **Usuario:** abre la pantalla de inicio de sesión.
2. **Sistema:** solicita el email y la contraseña.
3. **Usuario:** introduce sus credenciales.
4. **Sistema:** comprueba que el email pertenece al dominio corporativo de Turbine.
5. **Sistema:** verifica las credenciales y que la cuenta está activa.
6. **Sistema:** identifica el rol del usuario y sus clientes asignados.
7. **Sistema:** inicia la sesión y muestra la pantalla de trabajo diario (CU-23).

---

### 4. Flujos alternativos

#### FA-1 — Contraseña olvidada

**Se produce en:** paso 3 del flujo principal.  
**Condición:** el usuario no recuerda su contraseña.

1. El usuario selecciona "He olvidado mi contraseña".
2. Se ejecuta CU-02 (Recuperar contraseña).

**Continuación:** el caso de uso vuelve al paso 2 del flujo principal.

---

### 5. Flujos de excepción

#### FE-1 — Email fuera del dominio de Turbine

**Se produce en:** paso 4 del flujo principal.  
**Condición:** el email no pertenece al dominio corporativo.

1. El sistema rechaza el acceso.
2. El sistema informa de que solo se admiten cuentas corporativas.

**Resultado:** no se crea ninguna sesión.

#### FE-2 — Credenciales incorrectas

**Se produce en:** paso 5 del flujo principal.  
**Condición:** el email o la contraseña no son correctos.

1. El sistema muestra un error sin indicar cuál de los dos datos falla.
2. El sistema permite volver a intentarlo.

**Resultado:** el caso de uso vuelve al paso 3 del flujo principal.

#### FE-3 — Demasiados intentos fallidos

**Se produce en:** paso 5 del flujo principal.  
**Condición:** se han producido varios intentos fallidos seguidos.

1. El sistema bloquea temporalmente el acceso a esa cuenta.
2. El sistema informa al usuario del bloqueo.

**Resultado:** no se crea ninguna sesión hasta que termine el bloqueo.

#### FE-4 — Cuenta desactivada

**Se produce en:** paso 5 del flujo principal.  
**Condición:** la cuenta ha sido desactivada por un administrador.

1. El sistema deniega el acceso.

**Resultado:** no se crea ninguna sesión.

---

### 6. Postcondiciones

- POST-1. Existe una sesión activa con los permisos del rol del usuario.
- POST-2. La sesión se cerrará automáticamente tras un periodo de inactividad.

---

## CU-02 — Recuperar contraseña

### 1. Identificación

| Campo | Descripción |
|---|---|
| **Identificador** | CU-02 |
| **Nombre** | Recuperar contraseña |
| **Objetivo** | Permitir que un usuario que ha olvidado su contraseña establezca una nueva de forma segura. |
| **Actores participantes** | Usuario operativo (principal; también Administrador y Superadministrador por herencia), Servicio de mensajería (secundario) |
| **Relaciones** | Extiende CU-01 (Iniciar sesión) cuando el usuario ha olvidado la contraseña |
| **Requisitos relacionados** | UR-01; FR-03; NFR-16 |

---

### 2. Precondiciones

- PRE-1. El usuario está en la pantalla de inicio de sesión.

---

### 3. Flujo principal

1. **Usuario:** selecciona "He olvidado mi contraseña".
2. **Sistema:** solicita el email corporativo.
3. **Usuario:** introduce su email.
4. **Sistema:** genera un enlace de recuperación que caduca pasado un tiempo.
5. **Sistema:** envía el enlace a través del Servicio de mensajería.
6. **Usuario:** abre el enlace e introduce una nueva contraseña.
7. **Sistema:** guarda la nueva contraseña de forma segura e informa del cambio.

---

### 4. Flujos alternativos

#### FA-1 — Email sin cuenta asociada

**Se produce en:** paso 4 del flujo principal.  
**Condición:** el email introducido no corresponde a ninguna cuenta activa.

1. El sistema no genera ningún enlace.
2. El sistema muestra el mismo mensaje que en el caso correcto, para no revelar qué cuentas existen.

**Continuación:** el caso de uso termina.

---

### 5. Flujos de excepción

#### FE-1 — Enlace caducado

**Se produce en:** paso 6 del flujo principal.  
**Condición:** el usuario abre el enlace después de que haya caducado.

1. El sistema rechaza el enlace.
2. El sistema permite solicitar uno nuevo.

**Resultado:** el caso de uso vuelve al paso 2 del flujo principal.

#### FE-2 — Fallo en el envío

**Se produce en:** paso 5 del flujo principal.  
**Condición:** el Servicio de mensajería informa de que el envío ha fallado.

1. El sistema informa al usuario de que no se ha podido enviar el enlace.

**Resultado:** la contraseña no cambia; el usuario puede volver a intentarlo.

---

### 6. Postcondiciones

- POST-1. La contraseña queda actualizada y guardada de forma segura.
- POST-2. El enlace utilizado deja de ser válido.

---

## CU-03 — Gestionar usuarios y asignaciones

### 1. Identificación

| Campo | Descripción |
|---|---|
| **Identificador** | CU-03 |
| **Nombre** | Gestionar usuarios y asignaciones |
| **Objetivo** | Crear, modificar y desactivar cuentas de usuario, asignarles un rol y decidir qué clientes gestiona cada usuario operativo. |
| **Actores participantes** | Administrador (principal; también Superadministrador por herencia) |
| **Relaciones** | Ninguna |
| **Requisitos relacionados** | UR-02, UR-17; FR-05, FR-06, FR-07, FR-08, FR-74; RB-18 |

---

### 2. Precondiciones

- PRE-1. El administrador ha iniciado sesión (CU-01).

---

### 3. Flujo principal

1. **Administrador:** accede a la gestión de usuarios.
2. **Sistema:** muestra los usuarios existentes con su rol y su estado.
3. **Administrador:** selecciona crear un usuario nuevo.
4. **Administrador:** introduce el nombre, el email corporativo y el rol.
5. **Sistema:** comprueba que el email pertenece al dominio de Turbine.
6. **Administrador:** si es un usuario operativo, le asigna los clientes que va a gestionar.
7. **Sistema:** guarda el usuario y registra la acción en la auditoría.

---

### 4. Flujos alternativos

#### FA-1 — Modificar un usuario o sus asignaciones

**Se produce en:** paso 3 del flujo principal.  
**Condición:** el administrador quiere cambiar un usuario que ya existe.

1. El administrador selecciona el usuario.
2. El administrador cambia su rol o asigna y desasigna clientes.
3. Si cambia el responsable de un cliente, el nuevo responsable pasa a recibir sus avisos y el historial sigue indicando quién hizo cada acción anterior.

**Continuación:** el caso de uso continúa en el paso 7 del flujo principal.

#### FA-2 — Desactivar un usuario

**Se produce en:** paso 3 del flujo principal.  
**Condición:** un empleado deja Turbine o ya no debe acceder a la aplicación.

1. El administrador selecciona el usuario y lo desactiva.
2. El sistema cierra sus sesiones activas.
3. El sistema conserva el registro de todas sus acciones.

**Continuación:** el caso de uso continúa en el paso 7 del flujo principal.

---

### 5. Flujos de excepción

#### FE-1 — Datos incorrectos

**Se produce en:** paso 5 del flujo principal.  
**Condición:** faltan datos obligatorios o el email no es del dominio de Turbine.

1. El sistema indica qué datos hay que corregir.

**Resultado:** el caso de uso vuelve al paso 4 del flujo principal.

---

### 6. Postcondiciones

- POST-1. El usuario queda creado, modificado o desactivado, con su rol y sus clientes asignados.
- POST-2. El cambio queda registrado en la auditoría.

---

## CU-04 — Gestionar clientes y contactos

### 1. Identificación

| Campo | Descripción |
|---|---|
| **Identificador** | CU-04 |
| **Nombre** | Gestionar clientes y contactos |
| **Objetivo** | Registrar, consultar y mantener actualizadas las empresas cliente y sus personas de contacto. |
| **Actores participantes** | Usuario operativo (principal; también Administrador y Superadministrador por herencia) |
| **Relaciones** | Ninguna |
| **Requisitos relacionados** | UR-03, UR-10; FR-08 a FR-15, FR-56, FR-82; RB-01, RB-19 |

---

### 2. Precondiciones

- PRE-1. El usuario ha iniciado sesión (CU-01).

---

### 3. Flujo principal

1. **Usuario:** selecciona dar de alta un cliente.
2. **Sistema:** solicita el nombre comercial, la razón social, el identificador fiscal, la dirección fiscal, el país, la moneda, el sector y el responsable interno.
3. **Usuario:** introduce los datos.
4. **Sistema:** comprueba que no existe otro cliente con el mismo identificador fiscal.
5. **Usuario:** añade una o varias personas de contacto, indicando su función (facturación, impagos, etc.).
6. **Sistema:** valida los datos y guarda el cliente.

---

### 4. Flujos alternativos

#### FA-1 — Modificar un cliente

**Se produce en:** paso 1 del flujo principal.  
**Condición:** el usuario quiere cambiar los datos de un cliente que ya existe y que tiene asignado.

1. El usuario abre la ficha del cliente.
2. El usuario modifica los datos.
3. El sistema guarda los cambios y los registra en la auditoría.

**Continuación:** el caso de uso termina.

#### FA-2 — Marcar un contacto como inactivo

**Se produce en:** paso 5 del flujo principal.  
**Condición:** una persona de contacto ya no trabaja con el cliente.

1. El usuario marca el contacto como inactivo.
2. El sistema deja de enviarle comunicaciones, pero conserva el registro de las facturas que se le enviaron.

**Continuación:** el caso de uso continúa en el paso 6 del flujo principal.

#### FA-3 — Contacto que ya existe en otra empresa

**Se produce en:** paso 5 del flujo principal.  
**Condición:** la persona de contacto ya está registrada en otra empresa cliente.

1. El usuario busca el contacto existente.
2. El sistema lo asocia también a esta empresa, sin duplicarlo.

**Continuación:** el caso de uso continúa en el paso 6 del flujo principal.

#### FA-4 — Consultar el historial

**Se produce en:** paso 1 del flujo principal.  
**Condición:** el usuario quiere ver la evolución de un cliente.

1. El usuario abre la ficha del cliente y selecciona el historial.
2. El sistema muestra el alta, las facturas, los pagos, las incidencias, los cambios de tarifa y las bajas.

**Continuación:** el caso de uso termina.

#### FA-5 — Reactivar un cliente dado de baja

**Se produce en:** paso 4 del flujo principal.  
**Condición:** el identificador fiscal corresponde a un cliente dado de baja.

1. El sistema informa de que el cliente ya existió.
2. El usuario decide reactivarlo.
3. El sistema reutiliza su ficha anterior en lugar de crear un duplicado.

**Continuación:** el caso de uso continúa en el paso 5 del flujo principal.

---

### 5. Flujos de excepción

#### FE-1 — Cliente duplicado

**Se produce en:** paso 4 del flujo principal.  
**Condición:** ya existe un cliente activo con el mismo identificador fiscal.

1. El sistema avisa del duplicado.
2. El sistema permite abrir la ficha del cliente existente.

**Resultado:** no se crea un cliente nuevo.

#### FE-2 — Faltan datos obligatorios

**Se produce en:** paso 6 del flujo principal.  
**Condición:** algún dato obligatorio está vacío o no es válido.

1. El sistema indica qué campos hay que completar o corregir.

**Resultado:** el caso de uso vuelve al paso 3 del flujo principal.

#### FE-3 — Eliminar el único contacto de facturación

**Se produce en:** paso 5 del flujo principal (o en FA-1).  
**Condición:** se intenta eliminar el único contacto de facturación de un cliente activo.

1. El sistema avisa de que el cliente se quedaría sin contacto de facturación.
2. El usuario confirma o cancela la acción.

**Resultado:** el contacto solo se elimina si el usuario lo confirma.

#### FE-4 — Cliente no asignado

**Se produce en:** paso 1 de FA-1 o FA-4.  
**Condición:** un usuario operativo intenta abrir un cliente que no tiene asignado.

1. El sistema deniega el acceso.

**Resultado:** el caso de uso termina sin cambios.

---

### 6. Postcondiciones

- POST-1. El cliente y sus contactos quedan registrados o actualizados.
- POST-2. Ningún cliente se borra; las bajas conservan todos sus datos.

---

## CU-05 — Importar clientes desde CSV

### 1. Identificación

| Campo | Descripción |
|---|---|
| **Identificador** | CU-05 |
| **Nombre** | Importar clientes desde CSV |
| **Objetivo** | Dar de alta varios clientes a la vez a partir de un fichero CSV, sin introducir datos erróneos. |
| **Actores participantes** | Usuario operativo (principal; también Administrador y Superadministrador por herencia) |
| **Relaciones** | Ninguna |
| **Requisitos relacionados** | UR-04; FR-71, FR-72; A-19 |

---

### 2. Precondiciones

- PRE-1. El usuario ha iniciado sesión (CU-01).
- PRE-2. El usuario dispone de un fichero CSV con los datos de los clientes.

---

### 3. Flujo principal

1. **Usuario:** accede a la importación de clientes y selecciona el fichero.
2. **Sistema:** lee el fichero y muestra sus columnas.
3. **Usuario:** relaciona cada columna con el campo correspondiente del sistema.
4. **Sistema:** valida cada fila y muestra una vista previa, señalando las filas con errores y las que tienen un identificador fiscal duplicado.
5. **Usuario:** confirma la importación.
6. **Sistema:** importa solo las filas válidas.
7. **Sistema:** guarda y muestra un resumen con las filas creadas, las rechazadas y el motivo.

---

### 4. Flujos alternativos

#### FA-1 — Cancelar la importación

**Se produce en:** paso 5 del flujo principal.  
**Condición:** el usuario no está conforme con la vista previa.

1. El usuario cancela la importación.
2. El sistema no importa ningún cliente.

**Continuación:** el caso de uso termina.

---

### 5. Flujos de excepción

#### FE-1 — Fichero no válido

**Se produce en:** paso 2 del flujo principal.  
**Condición:** el fichero no tiene un formato CSV válido.

1. El sistema rechaza el fichero e indica el motivo.

**Resultado:** el caso de uso vuelve al paso 1 del flujo principal.

#### FE-2 — Ninguna fila válida

**Se produce en:** paso 4 del flujo principal.  
**Condición:** todas las filas tienen errores o están duplicadas.

1. El sistema informa de los errores encontrados.
2. El sistema no permite confirmar la importación.

**Resultado:** no se importa ningún cliente.

---

### 6. Postcondiciones

- POST-1. Los clientes válidos quedan creados, sin duplicar los existentes.
- POST-2. El resumen de la importación queda registrado.

---

## CU-06 — Gestionar suscripción (alta y pausa)

### 1. Identificación

| Campo | Descripción |
|---|---|
| **Identificador** | CU-06 |
| **Nombre** | Gestionar suscripción (alta y pausa) |
| **Objetivo** | Registrar la suscripción mensual de un cliente y pausarla temporalmente cuando se acuerde. |
| **Actores participantes** | Usuario operativo (principal; también Administrador y Superadministrador por herencia) |
| **Relaciones** | Extendido por CU-21 (Solicitar aprobación) cuando se registra una excepción comercial. Depende de CU-04 (el cliente debe existir) |
| **Requisitos relacionados** | UR-05, UR-10, UR-14; FR-16 a FR-20, FR-52, FR-81; RB-02, RB-05, RB-06, RB-13, RB-15, RB-20 |

---

### 2. Precondiciones

- PRE-1. El usuario ha iniciado sesión y tiene asignado al cliente.
- PRE-2. El cliente existe en el sistema (CU-04).

---

### 3. Flujo principal

1. **Usuario:** abre la ficha del cliente y selecciona crear la suscripción.
2. **Usuario:** indica la fecha de inicio, la tarifa mensual y las condiciones de pago.
3. **Sistema:** comprueba que el cliente tiene todos los datos fiscales obligatorios.
4. **Sistema:** calcula y muestra el importe prorrateado del primer mes (RB-05).
5. **Usuario:** revisa el importe y confirma el alta.
6. **Sistema:** guarda la suscripción y la activa.

---

### 4. Flujos alternativos

#### FA-1 — Fecha de inicio futura

**Se produce en:** paso 6 del flujo principal.  
**Condición:** la fecha de inicio es posterior a hoy.

1. El sistema guarda la suscripción y deja al cliente pendiente de activación.
2. El sistema no genera facturas hasta esa fecha.

**Continuación:** el caso de uso termina.

#### FA-2 — Condiciones de pago particulares

**Se produce en:** paso 2 del flujo principal.  
**Condición:** se ha pactado con el cliente un plazo de pago distinto del general.

1. El usuario indica el plazo pactado (por ejemplo, 30 días).
2. El sistema lo aplica a ese cliente por encima del plazo general (RB-06).

**Continuación:** el caso de uso continúa en el paso 3 del flujo principal.

#### FA-3 — Pausar la suscripción

**Se produce en:** paso 1 del flujo principal.  
**Condición:** el cliente tiene una suscripción activa y se acuerda pausarla.

1. El usuario selecciona pausar la suscripción.
2. El usuario indica el motivo, la fecha de inicio y la fecha prevista de reactivación.
3. El sistema pausa la suscripción; durante la pausa no se generan facturas nuevas, pero las anteriores siguen su cobro (RB-15).
4. Al llegar la fecha prevista, el sistema avisa al responsable para que confirme la reactivación.

**Continuación:** el caso de uso termina.

#### FA-4 — Excepción comercial

**Se produce en:** paso 2 del flujo principal (o con la suscripción ya activa).  
**Condición:** se ha pactado un descuento temporal, meses gratuitos o un precio especial.

1. El usuario registra la excepción comercial.
2. Se ejecuta CU-21 (Solicitar aprobación).
3. La excepción no se aplica hasta que se apruebe (CU-22).

**Continuación:** el caso de uso continúa en el paso 3 del flujo principal.

---

### 5. Flujos de excepción

#### FE-1 — Faltan datos fiscales

**Se produce en:** paso 3 del flujo principal.  
**Condición:** al cliente le falta algún dato fiscal obligatorio.

1. El sistema impide activar la suscripción.
2. El sistema indica qué datos faltan.

**Resultado:** la suscripción no se activa hasta que se completen los datos del cliente (CU-04).

---

### 6. Postcondiciones

- POST-1. La suscripción queda activa, pendiente de activación o pausada.
- POST-2. El planificador la tendrá en cuenta en la generación mensual (CU-09).

---

## CU-07 — Crear factura puntual

### 1. Identificación

| Campo | Descripción |
|---|---|
| **Identificador** | CU-07 |
| **Nombre** | Crear factura puntual |
| **Objetivo** | Preparar una factura por un servicio adicional (instalación, consultoría, etc.) que no forma parte de la suscripción. |
| **Actores participantes** | Usuario operativo (principal; también Administrador y Superadministrador por herencia) |
| **Relaciones** | Incluye CU-08 (Emitir factura) |
| **Requisitos relacionados** | UR-05, UR-06; FR-21, FR-22, FR-29, FR-80; RB-16 |

---

### 2. Precondiciones

- PRE-1. El usuario ha iniciado sesión y tiene asignado al cliente.

---

### 3. Flujo principal

1. **Usuario:** selecciona el cliente y la opción de crear una factura puntual.
2. **Usuario:** añade una o varias líneas con concepto, cantidad, precio, descuento e impuesto, eligiendo el impuesto entre los configurados.
3. **Sistema:** calcula los importes y guarda la factura como borrador.
4. **Usuario:** revisa la factura y decide emitirla.
5. **Sistema:** se ejecuta CU-08 (Emitir factura).

---

### 4. Flujos alternativos

#### FA-1 — Usar un concepto del catálogo

**Se produce en:** paso 2 del flujo principal.  
**Condición:** el servicio está en el catálogo de conceptos frecuentes.

1. El usuario elige el concepto del catálogo.
2. El sistema rellena el precio orientativo y el impuesto.
3. El usuario puede modificar el precio.

**Continuación:** el caso de uso continúa en el paso 3 del flujo principal.

#### FA-2 — Dejar la factura en borrador

**Se produce en:** paso 4 del flujo principal.  
**Condición:** el usuario quiere terminar la factura más tarde.

1. El usuario sale sin emitir la factura.
2. El sistema la conserva como borrador, que se puede modificar o eliminar.

**Continuación:** el caso de uso termina.

---

### 5. Flujos de excepción

#### FE-1 — Línea no válida

**Se produce en:** paso 3 del flujo principal.  
**Condición:** alguna línea tiene datos incorrectos (por ejemplo, una cantidad negativa).

1. El sistema indica qué línea y qué dato hay que corregir.

**Resultado:** el caso de uso vuelve al paso 2 del flujo principal.

---

### 6. Postcondiciones

- POST-1. La factura queda guardada como borrador, o emitida si se completa CU-08.

---

## CU-08 — Emitir factura

### 1. Identificación

| Campo | Descripción |
|---|---|
| **Identificador** | CU-08 |
| **Nombre** | Emitir factura |
| **Objetivo** | Convertir un borrador en una factura definitiva, con número y PDF, y enviarla al cliente. |
| **Actores participantes** | Usuario operativo (principal; también Administrador y Superadministrador por herencia). También se ejecuta incluido en CU-07 y CU-09 |
| **Relaciones** | Incluye CU-10 (Enviar factura). Extendido por CU-21 (Solicitar aprobación) |
| **Requisitos relacionados** | UR-05, UR-06; FR-27 a FR-32, FR-59; RB-10, RB-11, RB-13; NFR-11, NFR-13 |

---

### 2. Precondiciones

- PRE-1. Existe una factura en borrador.
- PRE-2. El cliente tiene al menos un contacto de facturación activo (RB-01).

---

### 3. Flujo principal

1. **Usuario:** solicita emitir la factura.
2. **Sistema:** muestra una vista previa de la factura.
3. **Sistema:** comprueba que no falta ningún dato fiscal obligatorio.
4. **Sistema:** comprueba si la factura necesita aprobación (RB-13).
5. **Usuario:** confirma la emisión.
6. **Sistema:** asigna el siguiente número único y correlativo de la serie.
7. **Sistema:** genera el PDF con la imagen corporativa de Turbine y lo guarda sin cambios.
8. **Sistema:** marca la factura como emitida y lo registra en la auditoría.
9. **Sistema:** se ejecuta CU-10 (Enviar factura).

---

### 4. Flujos alternativos

#### FA-1 — La factura necesita aprobación

**Se produce en:** paso 4 del flujo principal.  
**Condición:** la factura tiene un descuento, un importe modificado a mano o un importe superior al umbral configurado.

1. Se ejecuta CU-21 (Solicitar aprobación).
2. La factura queda pendiente de aprobación.
3. Cuando se aprueba (CU-22), la emisión puede continuar.

**Continuación:** el caso de uso continúa en el paso 5 del flujo principal.

---

### 5. Flujos de excepción

#### FE-1 — Falta un dato fiscal

**Se produce en:** paso 3 del flujo principal.  
**Condición:** falta algún dato fiscal obligatorio del cliente o de la factura.

1. El sistema impide la emisión.
2. El sistema indica qué dato falta.

**Resultado:** la factura sigue en borrador.

#### FE-2 — Doble pulsación o emisión simultánea

**Se produce en:** paso 6 del flujo principal.  
**Condición:** el usuario pulsa dos veces el botón de emitir, o dos usuarios emiten facturas a la vez.

1. El sistema procesa la emisión una sola vez.
2. El sistema garantiza que dos facturas no reciben el mismo número.

**Resultado:** no se crean facturas ni números duplicados.

#### FE-3 — Solicitud de aprobación rechazada

**Se produce en:** paso 3 de FA-1.  
**Condición:** el aprobador rechaza la solicitud.

1. El sistema informa al solicitante del rechazo y del comentario del aprobador.

**Resultado:** la factura vuelve a borrador y no se emite.

---

### 6. Postcondiciones

- POST-1. La factura tiene un número definitivo y un PDF que no se modificará.
- POST-2. La factura ya no puede editarse ni eliminarse; cualquier corrección se hará con CU-12.

---

## CU-09 — Generar facturas recurrentes

### 1. Identificación

| Campo | Descripción |
|---|---|
| **Identificador** | CU-09 |
| **Nombre** | Generar facturas recurrentes |
| **Objetivo** | Generar automáticamente cada mes las facturas de todas las suscripciones activas. |
| **Actores participantes** | Planificador del sistema (principal) |
| **Relaciones** | Incluye CU-08 (Emitir factura) |
| **Requisitos relacionados** | UR-05; FR-23 a FR-26, FR-75; RB-04, RB-05, RB-15, RB-20; NFR-11, NFR-12 |

---

### 2. Precondiciones

- PRE-1. Es día 1 de mes, o es la fecha de inicio de una nueva suscripción.

---

### 3. Flujo principal

1. **Planificador:** inicia el proceso de generación mensual.
2. **Sistema:** obtiene las suscripciones activas.
3. **Sistema:** para cada suscripción, crea la factura con la tarifa vigente en esa fecha.
4. **Sistema:** se ejecuta CU-08 (Emitir factura) para cada factura; las recurrentes normales no necesitan aprobación.
5. **Sistema:** registra la ejecución con los clientes procesados, las facturas creadas y los fallos.

---

### 4. Flujos alternativos

#### FA-1 — Alta a mitad de mes

**Se produce en:** paso 1 del flujo principal.  
**Condición:** hoy es la fecha de inicio de una suscripción nueva.

1. El sistema genera una factura proporcional por los días que quedan de mes (RB-05).

**Continuación:** el caso de uso continúa en el paso 4 del flujo principal.

#### FA-2 — Clientes que no deben facturarse

**Se produce en:** paso 3 del flujo principal.  
**Condición:** el cliente está pausado, pendiente de activación o dado de baja.

1. El sistema no genera factura para ese cliente.

**Continuación:** el caso de uso continúa con la siguiente suscripción en el paso 3.

---

### 5. Flujos de excepción

#### FE-1 — Falla la factura de un cliente

**Se produce en:** paso 3 o 4 del flujo principal.  
**Condición:** no se puede generar la factura de un cliente (por ejemplo, porque le falta un dato fiscal).

1. El sistema registra el error.
2. El sistema avisa al responsable de ese cliente.
3. El sistema continúa con el resto de clientes.

**Resultado:** las demás facturas se generan con normalidad.

#### FE-2 — El sistema estaba caído el día 1

**Se produce en:** paso 1 del flujo principal.  
**Condición:** el proceso no se pudo ejecutar el día 1.

1. Al volver a funcionar, el sistema detecta las facturas que faltan por generar.
2. El sistema las genera una única vez, sin duplicar las que ya existían.

**Resultado:** el caso de uso continúa en el paso 2 del flujo principal.

---

### 6. Postcondiciones

- POST-1. Cada suscripción activa tiene su factura del mes, emitida y enviada.
- POST-2. La ejecución queda registrada y puede consultarse en CU-27.

---

## CU-10 — Enviar factura

### 1. Identificación

| Campo | Descripción |
|---|---|
| **Identificador** | CU-10 |
| **Nombre** | Enviar factura |
| **Objetivo** | Hacer llegar la factura emitida a los contactos de facturación del cliente y registrar el resultado. |
| **Actores participantes** | Servicio de mensajería (secundario). Se ejecuta incluido en CU-08 |
| **Relaciones** | Incluido por CU-08 (Emitir factura) |
| **Requisitos relacionados** | UR-07; FR-35 a FR-38, FR-41; NFR-20, NFR-24 |

---

### 2. Precondiciones

- PRE-1. La factura está emitida y tiene su PDF.

---

### 3. Flujo principal

1. **Sistema:** prepara el mensaje con la plantilla correspondiente, en el idioma del cliente, incluyendo el email y el teléfono de soporte de Turbine.
2. **Sistema:** envía el mensaje con el PDF por email a los contactos de facturación.
3. **Sistema:** envía también un mensaje por WhatsApp a los contactos que han dado su consentimiento.
4. **Servicio de mensajería:** devuelve el resultado de cada envío.
5. **Sistema:** registra cada envío con su canal, destinatario, fecha y resultado.

---

### 4. Flujos alternativos

#### FA-1 — Reenviar la factura

**Se produce en:** después del paso 5 del flujo principal.  
**Condición:** el usuario ha corregido un contacto tras un envío fallido.

1. El usuario solicita reenviar la factura.
2. El sistema la reenvía como un nuevo intento, sin crear una factura nueva ni cambiar su número.

**Continuación:** el caso de uso continúa en el paso 4 del flujo principal.

---

### 5. Flujos de excepción

#### FE-1 — Envío fallido o correo rebotado

**Se produce en:** paso 4 del flujo principal.  
**Condición:** el Servicio de mensajería informa de que el envío ha fallado o el correo ha rebotado.

1. El sistema no da la factura por notificada.
2. El sistema avisa al responsable del cliente.
3. El sistema muestra el envío fallido en la pantalla de trabajo (CU-23).

**Resultado:** el envío queda registrado como fallido hasta que se reenvíe (FA-1).

---

### 6. Postcondiciones

- POST-1. Queda registrado a quién se envió la factura, por qué canal y con qué resultado.

---

## CU-11 — Enviar recordatorios

### 1. Identificación

| Campo | Descripción |
|---|---|
| **Identificador** | CU-11 |
| **Nombre** | Enviar recordatorios |
| **Objetivo** | Recordar automáticamente a los clientes sus facturas pendientes hasta que las paguen. |
| **Actores participantes** | Planificador del sistema (principal), Servicio de mensajería (secundario) |
| **Relaciones** | Ninguna |
| **Requisitos relacionados** | UR-07, UR-11; FR-39 a FR-41, FR-55, FR-62; RB-06, RB-07, RB-08, RB-14 |

---

### 2. Precondiciones

- PRE-1. Existen facturas emitidas que no están pagadas.

---

### 3. Flujo principal

1. **Planificador:** inicia cada día la revisión de facturas pendientes.
2. **Sistema:** calcula, para cada factura, si le corresponde un recordatorio según su vencimiento y las condiciones de pago del cliente (RB-06, RB-07).
3. **Sistema:** prepara el recordatorio con la plantilla correspondiente.
4. **Sistema:** lo envía a través del Servicio de mensajería.
5. **Servicio de mensajería:** devuelve el resultado del envío.
6. **Sistema:** registra el envío y su resultado.

---

### 4. Flujos alternativos

#### FA-1 — Factura vencida

**Se produce en:** paso 2 del flujo principal.  
**Condición:** la factura ha llegado a su vencimiento sin pagarse.

1. El sistema marca la factura como vencida.
2. El sistema avisa a su responsable interno.
3. El sistema sigue enviando recordatorios periódicos hasta que se pague (RB-08).

**Continuación:** el caso de uso continúa en el paso 3 del flujo principal.

#### FA-2 — Factura en disputa o con incidencia

**Se produce en:** paso 2 del flujo principal.  
**Condición:** los recordatorios de la factura están pausados (CU-15).

1. El sistema no envía recordatorios de esa factura hasta la fecha de revisión.

**Continuación:** el caso de uso continúa con la siguiente factura en el paso 2.

#### FA-3 — Cliente dado de baja con facturas pendientes

**Se produce en:** paso 2 del flujo principal.  
**Condición:** el cliente se ha dado de baja, pero tiene facturas sin pagar.

1. El sistema sigue enviando los recordatorios de esas facturas (RB-14).

**Continuación:** el caso de uso continúa en el paso 3 del flujo principal.

#### FA-4 — Factura pagada

**Se produce en:** paso 2 del flujo principal.  
**Condición:** la factura ya está pagada.

1. El sistema no envía más recordatorios de esa factura.

**Continuación:** el caso de uso continúa con la siguiente factura en el paso 2.

---

### 5. Flujos de excepción

#### FE-1 — Fallo en el envío

**Se produce en:** paso 5 del flujo principal.  
**Condición:** el Servicio de mensajería informa de que el envío ha fallado.

1. El sistema registra el fallo.
2. El sistema avisa al responsable del cliente.

**Resultado:** el resto de recordatorios se envían con normalidad.

---

### 6. Postcondiciones

- POST-1. Los recordatorios enviados y sus resultados quedan registrados.
- POST-2. Un impago nunca provoca la cancelación del servicio (RB-08).

---

## CU-12 — Crear factura rectificativa

### 1. Identificación

| Campo | Descripción |
|---|---|
| **Identificador** | CU-12 |
| **Nombre** | Crear factura rectificativa |
| **Objetivo** | Corregir una factura ya emitida sin modificarla, mediante una nueva factura vinculada a la original. |
| **Actores participantes** | Usuario operativo (principal; también Administrador y Superadministrador por herencia) |
| **Relaciones** | Incluye CU-21 (Solicitar aprobación). Depende de CU-08 (la factura original debe estar emitida) |
| **Requisitos relacionados** | UR-06; FR-29, FR-33, FR-48, FR-59; RB-10, RB-13; A-03 |

---

### 2. Precondiciones

- PRE-1. La factura original está emitida.
- PRE-2. El usuario tiene asignado al cliente.

---

### 3. Flujo principal

1. **Usuario:** abre la factura emitida y selecciona rectificarla.
2. **Sistema:** muestra los datos de la factura original.
3. **Usuario:** indica el motivo y los datos que se corrigen.
4. **Sistema:** prepara la factura rectificativa.
5. **Sistema:** se ejecuta CU-21 (Solicitar aprobación).
6. **Sistema:** cuando se aprueba, emite la rectificativa, la vincula con la original y la envía al cliente.

---

### 4. Flujos alternativos

#### FA-1 — Rectificación total (anulación)

**Se produce en:** paso 3 del flujo principal.  
**Condición:** la factura original debe anularse por completo.

1. El usuario indica que la rectificación es total.
2. El sistema prepara una rectificativa por el importe completo.

**Continuación:** el caso de uso continúa en el paso 5 del flujo principal.

---

### 5. Flujos de excepción

#### FE-1 — Solicitud rechazada

**Se produce en:** paso 5 del flujo principal.  
**Condición:** el aprobador rechaza la rectificativa (CU-22).

1. El sistema informa al usuario del rechazo.

**Resultado:** la rectificativa no se emite y la factura original no cambia.

#### FE-2 — Intento de editar una factura emitida

**Se produce en:** paso 1 del flujo principal.  
**Condición:** el usuario intenta modificar directamente la factura emitida.

1. El sistema no lo permite e indica que debe crear una rectificativa.

**Resultado:** el caso de uso continúa en el paso 1 del flujo principal.

---

### 6. Postcondiciones

- POST-1. La factura original se conserva intacta.
- POST-2. La rectificativa queda vinculada a la original y el saldo del cliente se recalcula.

---

## CU-13 — Conciliar pagos

### 1. Identificación

| Campo | Descripción |
|---|---|
| **Identificador** | CU-13 |
| **Nombre** | Conciliar pagos |
| **Objetivo** | Detectar automáticamente los pagos recibidos y asociarlos a sus facturas. |
| **Actores participantes** | Planificador del sistema (principal), Stripe simulado (secundario) |
| **Relaciones** | Extendido por CU-14 (Asociar pago manualmente) |
| **Requisitos relacionados** | UR-08, UR-09; FR-40, FR-42 a FR-46, FR-48, FR-62; RB-12; A-11; NFR-11, NFR-20, NFR-24 |

---

### 2. Precondiciones

- PRE-1. Existen facturas emitidas pendientes de pago.

---

### 3. Flujo principal

1. **Planificador:** inicia la consulta periódica de pagos.
2. **Stripe:** devuelve los pagos recibidos.
3. **Sistema:** para cada pago, busca la factura con la misma referencia e importe.
4. **Sistema:** si la coincidencia es clara, asocia el pago a la factura y la marca como pagada.
5. **Sistema:** actualiza el saldo del cliente y deja de enviar recordatorios de esa factura.
6. **Sistema:** avisa al responsable interno del cliente.

---

### 4. Flujos alternativos

#### FA-1 — Aviso de pago de Stripe

**Se produce en:** paso 1 del flujo principal.  
**Condición:** Stripe envía un aviso de pago sin esperar a la consulta periódica.

1. El sistema recibe el aviso con los datos del pago.

**Continuación:** el caso de uso continúa en el paso 3 del flujo principal.

#### FA-2 — Pago sin referencia clara

**Se produce en:** paso 3 del flujo principal.  
**Condición:** el pago no tiene referencia o puede corresponder a varias facturas.

1. El sistema deja el pago pendiente de asociar.
2. Se ejecuta CU-14 (Asociar pago manualmente).

**Continuación:** el caso de uso continúa con el siguiente pago en el paso 3.

#### FA-3 — Importe distinto al de la factura

**Se produce en:** paso 4 del flujo principal.  
**Condición:** el importe recibido es menor o mayor que el de la factura.

1. El sistema asocia el pago, pero no marca la factura como pagada.
2. El sistema registra la diferencia y la deja pendiente de revisión (RB-12).
3. Se ejecuta CU-14 (Asociar pago manualmente).

**Continuación:** el caso de uso continúa con el siguiente pago en el paso 3.

---

### 5. Flujos de excepción

#### FE-1 — Aviso de pago repetido

**Se produce en:** paso 3 del flujo principal.  
**Condición:** llega dos veces el aviso del mismo pago.

1. El sistema detecta que el pago ya está registrado y lo ignora.

**Resultado:** el pago no se cuenta dos veces.

#### FE-2 — Stripe no disponible

**Se produce en:** paso 2 del flujo principal.  
**Condición:** Stripe no responde.

1. El sistema registra el error.
2. El sistema no cambia el estado de ninguna factura.

**Resultado:** el sistema vuelve a intentarlo en la siguiente ejecución.

---

### 6. Postcondiciones

- POST-1. Los pagos claros quedan asociados a sus facturas, y de cada factura pagada se puede consultar el pago que lo justifica.
- POST-2. Los pagos dudosos y las diferencias de importe quedan pendientes de revisión manual.

---

## CU-14 — Asociar pago manualmente

### 1. Identificación

| Campo | Descripción |
|---|---|
| **Identificador** | CU-14 |
| **Nombre** | Asociar pago manualmente |
| **Objetivo** | Resolver los pagos que el sistema no ha podido asociar automáticamente. |
| **Actores participantes** | Usuario operativo (principal; también Administrador y Superadministrador por herencia) |
| **Relaciones** | Extiende CU-13 (Conciliar pagos) cuando el pago no tiene referencia clara o el importe no coincide |
| **Requisitos relacionados** | UR-09; FR-45, FR-46, FR-47; RB-12; NFR-20 |

---

### 2. Precondiciones

- PRE-1. Existe un pago pendiente de asociar o una diferencia de importe pendiente de revisión.
- PRE-2. El usuario tiene asignado al cliente.

---

### 3. Flujo principal

1. **Usuario:** abre la lista de pagos pendientes desde la pantalla de trabajo.
2. **Sistema:** muestra el pago y las facturas candidatas.
3. **Usuario:** selecciona la factura correcta e indica el motivo.
4. **Sistema:** asocia el pago y actualiza el estado de la factura y el saldo del cliente.
5. **Sistema:** registra quién hizo la asociación y por qué.

---

### 4. Flujos alternativos

#### FA-1 — Deshacer una asociación incorrecta

**Se produce en:** paso 1 del flujo principal.  
**Condición:** un pago se asoció a una factura equivocada.

1. El usuario selecciona la asociación y la deshace, indicando el motivo.
2. El sistema no borra el registro, sino que guarda quién la deshizo y por qué.

**Continuación:** el caso de uso continúa en el paso 2 del flujo principal.

#### FA-2 — Diferencia de importe

**Se produce en:** paso 4 del flujo principal.  
**Condición:** el importe del pago no coincide con el de la factura.

1. El usuario revisa la diferencia y deja constancia de ella.
2. La factura no se marca como pagada hasta que esté cubierta por completo.

**Continuación:** el caso de uso continúa en el paso 5 del flujo principal.

---

### 5. Flujos de excepción

#### FE-1 — Cliente no asignado

**Se produce en:** paso 3 del flujo principal.  
**Condición:** el usuario operativo no tiene asignado al cliente de la factura.

1. El sistema deniega la operación.

**Resultado:** el pago sigue pendiente.

---

### 6. Postcondiciones

- POST-1. El pago queda asociado, o la asociación incorrecta queda deshecha, con su registro completo.

---

## CU-15 — Gestionar disputas e incidencias

### 1. Identificación

| Campo | Descripción |
|---|---|
| **Identificador** | CU-15 |
| **Nombre** | Gestionar disputas e incidencias |
| **Objetivo** | Registrar y seguir los problemas de cobro, pausando los recordatorios cuando sea necesario. |
| **Actores participantes** | Usuario operativo (principal; también Administrador y Superadministrador por herencia) |
| **Relaciones** | Ninguna |
| **Requisitos relacionados** | UR-09, UR-11; FR-49, FR-50, FR-62 |

---

### 2. Precondiciones

- PRE-1. El usuario ha iniciado sesión y tiene asignado al cliente.

---

### 3. Flujo principal

1. **Usuario:** abre una incidencia sobre un cliente y, si procede, sobre una factura.
2. **Usuario:** indica el tipo (por ejemplo, "cliente reclama importe" o "pago no localizado") y el responsable.
3. **Sistema:** guarda la incidencia como abierta.
4. **Usuario:** añade comentarios internos durante el seguimiento.
5. **Usuario:** cierra la incidencia indicando la resolución.
6. **Sistema:** guarda la resolución.

---

### 4. Flujos alternativos

#### FA-1 — Marcar una factura en disputa

**Se produce en:** paso 1 del flujo principal.  
**Condición:** el cliente cuestiona una factura.

1. El usuario marca la factura en disputa, con un motivo y una fecha de revisión.
2. El sistema pausa sus recordatorios, pero la sigue mostrando como no cobrada.
3. Al llegar la fecha de revisión, el sistema avisa al responsable.

**Continuación:** el caso de uso continúa en el paso 4 del flujo principal.

#### FA-2 — Pausar los recordatorios por una incidencia

**Se produce en:** paso 3 del flujo principal.  
**Condición:** la incidencia afecta al cobro de una factura.

1. El usuario indica que se pausen los recordatorios de la factura.
2. El sistema deja de enviarlos mientras la incidencia esté abierta.

**Continuación:** el caso de uso continúa en el paso 4 del flujo principal.

---

### 5. Flujos de excepción

#### FE-1 — Faltan datos obligatorios

**Se produce en:** paso 3 del flujo principal (o en FA-1).  
**Condición:** falta el tipo, el motivo o la fecha de revisión.

1. El sistema indica qué datos faltan.

**Resultado:** la incidencia o la disputa no se guarda hasta completar los datos.

---

### 6. Postcondiciones

- POST-1. La incidencia o la disputa queda registrada con su estado.
- POST-2. Al cerrarse, queda guardada su resolución.

---

## CU-16 — Declarar factura incobrable

### 1. Identificación

| Campo | Descripción |
|---|---|
| **Identificador** | CU-16 |
| **Nombre** | Declarar factura incobrable |
| **Objetivo** | Dar por imposible de cobrar una factura, de forma controlada y sin marcarla como pagada. |
| **Actores participantes** | Administrador (principal; también Superadministrador por herencia) |
| **Relaciones** | Incluye CU-21 (Solicitar aprobación) |
| **Requisitos relacionados** | UR-09, UR-14; FR-51, FR-74; RB-12, RB-13 |

---

### 2. Precondiciones

- PRE-1. La factura está emitida y no está pagada.

---

### 3. Flujo principal

1. **Administrador:** abre la factura y selecciona declararla incobrable.
2. **Administrador:** indica el motivo.
3. **Sistema:** se ejecuta CU-21 (Solicitar aprobación).
4. **Sistema:** cuando se aprueba, marca la factura como incobrable y detiene sus recordatorios.
5. **Sistema:** registra la acción en la auditoría.

---

### 4. Flujos alternativos

No se identifican flujos alternativos relevantes.

---

### 5. Flujos de excepción

#### FE-1 — Falta el motivo

**Se produce en:** paso 2 del flujo principal.  
**Condición:** el administrador no indica el motivo.

1. El sistema no permite enviar la solicitud.

**Resultado:** el caso de uso vuelve al paso 2 del flujo principal.

#### FE-2 — Solicitud rechazada

**Se produce en:** paso 3 del flujo principal.  
**Condición:** el aprobador rechaza la solicitud (CU-22).

1. El sistema informa al administrador del rechazo.

**Resultado:** la factura sigue pendiente y continúa su proceso de cobro.

---

### 6. Postcondiciones

- POST-1. La factura queda como incobrable, que es un estado distinto de "pagada" (RB-12).
- POST-2. La acción queda registrada en la auditoría.

---

## CU-17 — Gestionar baja de cliente

### 1. Identificación

| Campo | Descripción |
|---|---|
| **Identificador** | CU-17 |
| **Nombre** | Gestionar baja de cliente |
| **Objetivo** | Dar de baja la suscripción de un cliente, dejando de facturarle sin perder sus deudas ni su historial. |
| **Actores participantes** | Usuario operativo (principal; también Administrador y Superadministrador por herencia). También se ejecuta incluido en CU-18 |
| **Relaciones** | Incluido por CU-18 (Comunicar solicitud de baja). Depende de CU-06 (el cliente debe tener una suscripción) |
| **Requisitos relacionados** | UR-10; FR-12, FR-53, FR-54, FR-55; RB-14, RB-19, RB-20; Q-01 |

---

### 2. Precondiciones

- PRE-1. El cliente tiene una suscripción activa o pausada.
- PRE-2. El usuario tiene asignado al cliente.

---

### 3. Flujo principal

1. **Usuario:** registra la solicitud de baja del cliente e indica la fecha efectiva.
2. **Sistema:** comprueba si el cliente tiene facturas pendientes.
3. **Sistema:** muestra al usuario las consecuencias de la baja.
4. **Usuario:** confirma la baja.
5. **Sistema:** deja al cliente en estado "baja solicitada" hasta la fecha efectiva.
6. **Sistema:** en la fecha efectiva, cierra la suscripción y deja de generar facturas recurrentes.
7. **Sistema:** conserva la ficha y el historial del cliente.

---

### 4. Flujos alternativos

#### FA-1 — Cliente con facturas pendientes

**Se produce en:** paso 2 del flujo principal.  
**Condición:** el cliente tiene facturas sin pagar.

1. El sistema informa de las facturas pendientes.
2. La baja se realiza igualmente, pero el sistema mantiene los recordatorios de esas facturas hasta que se paguen (RB-14).

**Continuación:** el caso de uso continúa en el paso 3 del flujo principal.

#### FA-2 — Cancelar la baja

**Se produce en:** paso 4 del flujo principal.  
**Condición:** el usuario decide no continuar.

1. El usuario cancela la operación.
2. El sistema no realiza ningún cambio.

**Continuación:** el caso de uso termina.

---

### 5. Flujos de excepción

#### FE-1 — Cliente no asignado

**Se produce en:** paso 1 del flujo principal.  
**Condición:** el usuario operativo no tiene asignado al cliente.

1. El sistema deniega la operación.

**Resultado:** el caso de uso termina sin cambios.

---

### 6. Postcondiciones

- POST-1. El cliente no recibe nuevas facturas recurrentes desde la fecha de baja.
- POST-2. Sus facturas pendientes se siguen reclamando y su historial se conserva.

---

## CU-18 — Comunicar solicitud de baja (API)

### 1. Identificación

| Campo | Descripción |
|---|---|
| **Identificador** | CU-18 |
| **Nombre** | Comunicar solicitud de baja (API) |
| **Objetivo** | Permitir que otro sistema de Turbine comunique que un cliente quiere darse de baja. |
| **Actores participantes** | Sistema externo vía API (principal) |
| **Relaciones** | Incluye CU-17 (Gestionar baja de cliente) |
| **Requisitos relacionados** | UR-18; FR-53, FR-77, FR-78; NFR-15, NFR-25 |

---

### 2. Precondiciones

- PRE-1. El sistema externo dispone de credenciales válidas para la API.

---

### 3. Flujo principal

1. **Sistema externo:** envía a la API la solicitud de baja de un cliente.
2. **Sistema:** comprueba la autenticación y los permisos de la llamada.
3. **Sistema:** registra la solicitud y avisa al responsable del cliente.
4. **Sistema:** devuelve que la solicitud ha sido registrada.
5. **Sistema:** se ejecuta CU-17 (Gestionar baja de cliente): el responsable revisa la solicitud y confirma la baja con las reglas de la aplicación.

---

### 4. Flujos alternativos

#### FA-1 — Cliente con facturas pendientes

**Se produce en:** paso 4 del flujo principal.  
**Condición:** el cliente tiene facturas sin pagar.

1. La API indica que la baja se ha registrado, pero que hay facturas pendientes que se seguirán reclamando (RB-14).

**Continuación:** el caso de uso continúa en el paso 5 del flujo principal.

---

### 5. Flujos de excepción

#### FE-1 — Credenciales no válidas

**Se produce en:** paso 2 del flujo principal.  
**Condición:** la llamada no tiene credenciales válidas o no tiene permiso.

1. La API rechaza la petición.

**Resultado:** no se registra ninguna solicitud.

#### FE-2 — Cliente inexistente

**Se produce en:** paso 3 del flujo principal.  
**Condición:** el cliente indicado no existe.

1. La API devuelve un error.

**Resultado:** no se registra ninguna solicitud.

---

### 6. Postcondiciones

- POST-1. La solicitud de baja queda registrada y se gestiona con las mismas reglas que una baja manual.
- POST-2. El sistema externo no puede cerrar la suscripción directamente.

---

## CU-19 — Consultar facturas pendientes (API)

### 1. Identificación

| Campo | Descripción |
|---|---|
| **Identificador** | CU-19 |
| **Nombre** | Consultar facturas pendientes (API) |
| **Objetivo** | Permitir que otro sistema de Turbine sepa si un cliente tiene facturas pendientes de pago. |
| **Actores participantes** | Sistema externo vía API (principal) |
| **Relaciones** | Ninguna |
| **Requisitos relacionados** | UR-18; FR-76, FR-78; NFR-15, NFR-25 |

---

### 2. Precondiciones

- PRE-1. El sistema externo dispone de credenciales válidas para la API.

---

### 3. Flujo principal

1. **Sistema externo:** envía a la API el identificador de un cliente.
2. **Sistema:** comprueba la autenticación y los permisos de la llamada.
3. **Sistema:** calcula si el cliente tiene facturas pendientes.
4. **Sistema:** devuelve si hay facturas pendientes, cuántas son y el importe pendiente.
5. **Sistema:** registra la llamada.

---

### 4. Flujos alternativos

#### FA-1 — Cliente sin facturas pendientes

**Se produce en:** paso 3 del flujo principal.  
**Condición:** el cliente no tiene ninguna factura pendiente.

1. La API indica que no hay facturas pendientes.

**Continuación:** el caso de uso continúa en el paso 5 del flujo principal.

---

### 5. Flujos de excepción

#### FE-1 — Credenciales no válidas

**Se produce en:** paso 2 del flujo principal.  
**Condición:** la llamada no tiene credenciales válidas o no tiene permiso.

1. La API rechaza la petición.

**Resultado:** no se devuelve ninguna información.

#### FE-2 — Cliente inexistente

**Se produce en:** paso 3 del flujo principal.  
**Condición:** el cliente indicado no existe.

1. La API devuelve un error.

**Resultado:** el caso de uso termina.

---

### 6. Postcondiciones

- POST-1. La consulta no modifica ningún dato.
- POST-2. La llamada queda registrada.

---

## CU-20 — Revisar tarifa anual

### 1. Identificación

| Campo | Descripción |
|---|---|
| **Identificador** | CU-20 |
| **Nombre** | Revisar tarifa anual |
| **Objetivo** | Revisar una vez al año el rango de tarifa de cada cliente según su facturación anual. |
| **Actores participantes** | Administrador (principal; también Superadministrador por herencia) |
| **Relaciones** | Incluye CU-21 (Solicitar aprobación) |
| **Requisitos relacionados** | UR-15; FR-57, FR-58; RB-02, RB-03; A-09 |

---

### 2. Precondiciones

- PRE-1. Ha llegado el periodo de revisión anual.
- PRE-2. Los rangos de tarifa están configurados (CU-26).

---

### 3. Flujo principal

1. **Administrador:** abre la revisión anual de tarifas.
2. **Sistema:** muestra la lista de clientes cuyo rango debe revisarse.
3. **Administrador:** selecciona un cliente e introduce su facturación anual, indicando la fuente y la fecha del dato.
4. **Sistema:** propone el rango y el precio que le corresponden.
5. **Sistema:** si el rango cambia, se ejecuta CU-21 (Solicitar aprobación).
6. **Sistema:** cuando se aprueba, aplica el nuevo precio desde la fecha de efecto y registra el precio anterior, el nuevo y quién lo aprobó.

---

### 4. Flujos alternativos

#### FA-1 — El rango no cambia

**Se produce en:** paso 4 del flujo principal.  
**Condición:** la facturación del cliente sigue en el mismo rango.

1. El sistema registra la revisión sin modificar la tarifa.

**Continuación:** el caso de uso termina.

#### FA-2 — Dato de facturación no disponible

**Se produce en:** paso 3 del flujo principal.  
**Condición:** el administrador todavía no tiene la facturación anual del cliente.

1. El sistema deja la revisión de ese cliente pendiente.

**Continuación:** el caso de uso continúa con otro cliente en el paso 3.

---

### 5. Flujos de excepción

#### FE-1 — Cambio rechazado

**Se produce en:** paso 5 del flujo principal.  
**Condición:** el aprobador rechaza el cambio de tarifa (CU-22).

1. El sistema mantiene la tarifa anterior.

**Resultado:** la revisión queda registrada como rechazada.

---

### 6. Postcondiciones

- POST-1. La revisión queda registrada.
- POST-2. Las facturas anteriores no se modifican y no se generan facturas retroactivas.

---

## CU-21 — Solicitar aprobación

### 1. Identificación

| Campo | Descripción |
|---|---|
| **Identificador** | CU-21 |
| **Nombre** | Solicitar aprobación |
| **Objetivo** | Dejar pendiente de aprobación una operación económica sensible hasta que la valide un administrador. |
| **Actores participantes** | Ninguno directo: se ejecuta dentro de otros casos de uso |
| **Relaciones** | Incluido por CU-12, CU-16 y CU-20. Extiende CU-06 y CU-08 |
| **Requisitos relacionados** | UR-14; FR-59, FR-60, FR-81; RB-13; A-12 |

---

### 2. Precondiciones

- PRE-1. La operación es de las que requieren aprobación (RB-13).

---

### 3. Flujo principal

1. **Sistema:** crea la solicitud con la operación, el solicitante y el motivo.
2. **Sistema:** indica al solicitante por qué la operación queda pendiente.
3. **Sistema:** añade la solicitud a la bandeja de aprobaciones y avisa a los aprobadores.
4. **Sistema:** deja la operación en espera hasta que se resuelva mediante CU-22.

---

### 4. Flujos alternativos

#### FA-1 — Importe por encima del umbral de separación de funciones

**Se produce en:** paso 3 del flujo principal.  
**Condición:** el importe supera el umbral configurado de separación de funciones.

1. El sistema marca la solicitud para que no pueda aprobarla el propio solicitante (FR-61).

**Continuación:** el caso de uso continúa en el paso 4 del flujo principal.

---

### 5. Flujos de excepción

#### FE-1 — Falta el motivo

**Se produce en:** paso 1 del flujo principal.  
**Condición:** el solicitante no ha indicado el motivo de la operación.

1. El sistema pide el motivo.

**Resultado:** la solicitud no se crea hasta que se indique.

---

### 6. Postcondiciones

- POST-1. La operación no se aplica hasta que se apruebe.
- POST-2. El solicitante puede consultar el estado de su solicitud en la bandeja.

---

## CU-22 — Aprobar o rechazar solicitudes

### 1. Identificación

| Campo | Descripción |
|---|---|
| **Identificador** | CU-22 |
| **Nombre** | Aprobar o rechazar solicitudes |
| **Objetivo** | Revisar y resolver las operaciones económicas pendientes de aprobación. |
| **Actores participantes** | Administrador (principal; también Superadministrador por herencia) |
| **Relaciones** | Depende de CU-21 (debe existir una solicitud de aprobación) |
| **Requisitos relacionados** | UR-14; FR-60, FR-61, FR-62, FR-74; RB-13; A-12 |

---

### 2. Precondiciones

- PRE-1. Existen solicitudes pendientes en la bandeja de aprobaciones.

---

### 3. Flujo principal

1. **Administrador:** abre la bandeja de aprobaciones.
2. **Sistema:** muestra las solicitudes pendientes con su motivo.
3. **Administrador:** selecciona una solicitud y revisa su detalle.
4. **Administrador:** la aprueba y añade un comentario.
5. **Sistema:** aplica la operación.
6. **Sistema:** registra la decisión en la auditoría y avisa al solicitante.

---

### 4. Flujos alternativos

#### FA-1 — Rechazar la solicitud

**Se produce en:** paso 4 del flujo principal.  
**Condición:** el administrador no está de acuerdo con la operación.

1. El administrador rechaza la solicitud y añade un comentario.
2. El sistema descarta la operación.

**Continuación:** el caso de uso continúa en el paso 6 del flujo principal.

---

### 5. Flujos de excepción

#### FE-1 — Aprobar su propia solicitud

**Se produce en:** paso 4 del flujo principal.  
**Condición:** el administrador es el solicitante y el importe supera el umbral de separación de funciones.

1. El sistema no permite la aprobación.
2. El sistema indica que debe aprobarla otra persona.

**Resultado:** la solicitud sigue pendiente.

---

### 6. Postcondiciones

- POST-1. La solicitud queda aprobada o rechazada, con el comentario y el aprobador registrados.
- POST-2. El solicitante puede ver el resultado en su bandeja.

---

## CU-23 — Consultar pantalla de trabajo y notificaciones

### 1. Identificación

| Campo | Descripción |
|---|---|
| **Identificador** | CU-23 |
| **Nombre** | Consultar pantalla de trabajo y notificaciones |
| **Objetivo** | Ver de un vistazo lo que hay que resolver hoy y los avisos de los clientes asignados. |
| **Actores participantes** | Usuario operativo (principal; también Administrador y Superadministrador por herencia) |
| **Relaciones** | Ninguna |
| **Requisitos relacionados** | UR-11; FR-62, FR-63 |

---

### 2. Precondiciones

- PRE-1. El usuario ha iniciado sesión (CU-01).

---

### 3. Flujo principal

1. **Usuario:** entra en la aplicación.
2. **Sistema:** muestra la pantalla de trabajo diario con las facturas que vencen pronto, las vencidas, los pagos pendientes de asociar, los envíos fallidos y las aprobaciones pendientes de sus clientes.
3. **Sistema:** muestra las notificaciones del usuario (pagos recibidos, facturas vencidas, disputas, revisiones anuales y aprobaciones resueltas).
4. **Usuario:** selecciona un elemento.
5. **Sistema:** lleva al usuario directamente a ese elemento para resolverlo.

---

### 4. Flujos alternativos

#### FA-1 — Marcar una notificación

**Se produce en:** paso 4 del flujo principal.  
**Condición:** el usuario ya ha visto o resuelto el aviso.

1. El usuario marca la notificación como leída o como resuelta.
2. El sistema actualiza su estado.

**Continuación:** el caso de uso vuelve al paso 3 del flujo principal.

#### FA-2 — Vista de administrador

**Se produce en:** paso 2 del flujo principal.  
**Condición:** el usuario es Administrador o Superadministrador.

1. El sistema muestra la información de todos los clientes, no solo de los asignados.

**Continuación:** el caso de uso continúa en el paso 3 del flujo principal.

#### FA-3 — Nada pendiente

**Se produce en:** paso 2 del flujo principal.  
**Condición:** no hay elementos pendientes.

1. El sistema indica que no hay tareas pendientes.

**Continuación:** el caso de uso termina.

---

### 5. Flujos de excepción

No se identifican flujos de excepción relevantes.

---

### 6. Postcondiciones

- POST-1. La consulta no modifica datos, salvo el estado de las notificaciones.

---

## CU-24 — Buscar clientes y facturas

### 1. Identificación

| Campo | Descripción |
|---|---|
| **Identificador** | CU-24 |
| **Nombre** | Buscar clientes y facturas |
| **Objetivo** | Encontrar rápidamente cualquier cliente o factura. |
| **Actores participantes** | Usuario operativo (principal; también Administrador y Superadministrador por herencia) |
| **Relaciones** | Ninguna |
| **Requisitos relacionados** | UR-08, UR-12; FR-08, FR-34, FR-69, FR-70, FR-79; RB-20; NFR-08 |

---

### 2. Precondiciones

- PRE-1. El usuario ha iniciado sesión (CU-01).

---

### 3. Flujo principal

1. **Usuario:** escribe en la búsqueda global parte del nombre, la razón social, el identificador fiscal, una persona de contacto, un email o un número de factura.
2. **Sistema:** muestra los resultados que el usuario tiene permiso para ver.
3. **Usuario:** selecciona un resultado.
4. **Sistema:** muestra la ficha del cliente o de la factura.

---

### 4. Flujos alternativos

#### FA-1 — Filtrar clientes

**Se produce en:** paso 1 del flujo principal.  
**Condición:** el usuario quiere ver el listado de clientes.

1. El usuario filtra por ubicación, sector o estado.
2. El sistema muestra el listado con el estado de cada cliente (activo, pausado, baja solicitada…) sin necesidad de abrir su ficha.

**Continuación:** el caso de uso continúa en el paso 3 del flujo principal.

#### FA-2 — Filtrar facturas

**Se produce en:** paso 1 del flujo principal.  
**Condición:** el usuario quiere ver un listado de facturas.

1. El usuario filtra por estado, tipo (puntual o recurrente), cliente o fecha.
2. El sistema muestra las facturas que cumplen el filtro.

**Continuación:** el caso de uso continúa en el paso 3 del flujo principal.

#### FA-3 — Sin resultados

**Se produce en:** paso 2 del flujo principal.  
**Condición:** ningún cliente ni factura coincide con la búsqueda.

1. El sistema indica que no hay resultados.

**Continuación:** el caso de uso vuelve al paso 1 del flujo principal.

---

### 5. Flujos de excepción

#### FE-1 — Acceso no permitido

**Se produce en:** paso 3 del flujo principal.  
**Condición:** un usuario operativo intenta abrir un cliente o una factura que no tiene asignado (por ejemplo, escribiendo la dirección web).

1. El sistema deniega el acceso.

**Resultado:** no se muestra la ficha.

---

### 6. Postcondiciones

- POST-1. La búsqueda no modifica ningún dato.

---

## CU-25 — Consultar dashboard e informes

### 1. Identificación

| Campo | Descripción |
|---|---|
| **Identificador** | CU-25 |
| **Nombre** | Consultar dashboard e informes |
| **Objetivo** | Conocer la situación financiera de Turbine y exportar informes. |
| **Actores participantes** | Administrador (principal; también Superadministrador por herencia) |
| **Relaciones** | Ninguna |
| **Requisitos relacionados** | UR-13; FR-64 a FR-68; RB-21; A-07, A-17; NFR-06, NFR-09, NFR-14 |

---

### 2. Precondiciones

- PRE-1. El usuario ha iniciado sesión como Administrador o Superadministrador.

---

### 3. Flujo principal

1. **Administrador:** abre el dashboard.
2. **Sistema:** muestra lo facturado, cobrado, pendiente y vencido por semana, mes, trimestre y año, el tiempo medio de cobro, los clientes con mayor deuda y la previsión de ingresos recurrentes del mes siguiente.
3. **Administrador:** pulsa una cifra.
4. **Sistema:** muestra las facturas que la componen.
5. **Administrador:** genera un informe filtrando por periodo, cliente, responsable, país, sector, estado o tipo de factura.
6. **Administrador:** exporta el informe a CSV o PDF.
7. **Sistema:** genera el fichero solo con los datos que el usuario puede ver.

---

### 4. Flujos alternativos

#### FA-1 — Paquete para la gestoría

**Se produce en:** paso 5 del flujo principal.  
**Condición:** el administrador necesita enviar la información del periodo a la gestoría.

1. El administrador selecciona el mes o el trimestre.
2. El sistema genera un archivo comprimido con las facturas emitidas, las rectificativas, los cobros y sus PDF (A-17).

**Continuación:** el caso de uso termina.

#### FA-2 — Informe muy grande

**Se produce en:** paso 7 del flujo principal.  
**Condición:** el informe tiene muchos datos.

1. El sistema lo genera en segundo plano, sin bloquear la aplicación.
2. El sistema avisa al administrador cuando está listo.

**Continuación:** el caso de uso termina.

---

### 5. Flujos de excepción

#### FE-1 — Acceso de un usuario operativo

**Se produce en:** paso 1 del flujo principal.  
**Condición:** un usuario operativo intenta abrir el dashboard.

1. El sistema deniega el acceso.

**Resultado:** el caso de uso termina.

#### FE-2 — Sin datos en el periodo

**Se produce en:** paso 2 o 5 del flujo principal.  
**Condición:** no hay datos para el periodo elegido.

1. El sistema indica que no hay datos.

**Resultado:** el administrador puede elegir otro periodo.

---

### 6. Postcondiciones

- POST-1. La consulta no modifica ningún dato.
- POST-2. Las exportaciones incluyen solo los datos que el usuario tiene permiso para ver.

---

## CU-26 — Configurar el sistema

### 1. Identificación

| Campo | Descripción |
|---|---|
| **Identificador** | CU-26 |
| **Nombre** | Configurar el sistema |
| **Objetivo** | Modificar los parámetros de negocio sin tocar el código. |
| **Actores participantes** | Superadministrador (principal) |
| **Relaciones** | Ninguna |
| **Requisitos relacionados** | UR-16; FR-41, FR-73, FR-74; RB-17 |

---

### 2. Precondiciones

- PRE-1. El usuario ha iniciado sesión como Superadministrador.

---

### 3. Flujo principal

1. **Superadministrador:** abre la configuración.
2. **Sistema:** muestra los parámetros actuales: plazo de pago por defecto, cadencia de recordatorios, dominio de correo, datos fiscales de Turbine, logo, series de facturación, impuestos, rangos de tarifa y umbrales de aprobación.
3. **Superadministrador:** modifica uno o varios parámetros.
4. **Sistema:** valida que los valores son coherentes.
5. **Sistema:** guarda los cambios y los registra en la auditoría.

---

### 4. Flujos alternativos

#### FA-1 — Editar las plantillas de mensaje

**Se produce en:** paso 3 del flujo principal.  
**Condición:** el superadministrador quiere cambiar el texto de las facturas o recordatorios.

1. El superadministrador edita la plantilla en español o en inglés.
2. El sistema guarda la nueva versión.

**Continuación:** el caso de uso continúa en el paso 5 del flujo principal.

---

### 5. Flujos de excepción

#### FE-1 — Valor no válido

**Se produce en:** paso 4 del flujo principal.  
**Condición:** algún valor no es válido (por ejemplo, un plazo negativo o una serie duplicada).

1. El sistema no guarda los cambios.
2. El sistema indica qué valor es incorrecto.

**Resultado:** el caso de uso vuelve al paso 3 del flujo principal.

#### FE-2 — Usuario sin permiso

**Se produce en:** paso 1 del flujo principal.  
**Condición:** un usuario que no es Superadministrador intenta abrir la configuración.

1. El sistema deniega el acceso.

**Resultado:** el caso de uso termina.

---

### 6. Postcondiciones

- POST-1. La nueva configuración se aplica solo a partir de ese momento; las facturas ya emitidas no cambian (RB-17).
- POST-2. El cambio queda registrado en la auditoría.

---

## CU-27 — Consultar auditoría y estado del sistema

### 1. Identificación

| Campo | Descripción |
|---|---|
| **Identificador** | CU-27 |
| **Nombre** | Consultar auditoría y estado del sistema |
| **Objetivo** | Saber quién hizo cada acción y comprobar que los procesos automáticos funcionan. |
| **Actores participantes** | Superadministrador (principal) |
| **Relaciones** | Ninguna |
| **Requisitos relacionados** | UR-17; FR-74, FR-75; NFR-21, NFR-22 |

---

### 2. Precondiciones

- PRE-1. El usuario ha iniciado sesión como Superadministrador.

---

### 3. Flujo principal

1. **Superadministrador:** abre la auditoría.
2. **Sistema:** muestra las acciones registradas: quién, qué y cuándo.
3. **Superadministrador:** filtra por usuario, tipo de acción, cliente o fecha.
4. **Sistema:** muestra los resultados filtrados.

---

### 4. Flujos alternativos

#### FA-1 — Consultar el estado del sistema

**Se produce en:** paso 1 del flujo principal.  
**Condición:** el superadministrador quiere comprobar los procesos automáticos.

1. El superadministrador abre la pantalla de estado.
2. El sistema muestra la última generación de facturas, los mensajes pendientes de enviar, los errores recientes y la última copia de seguridad.
3. El superadministrador puede abrir el detalle de una ejecución: elementos procesados, fallos y su motivo.

**Continuación:** el caso de uso termina.

---

### 5. Flujos de excepción

#### FE-1 — Intento de modificar la auditoría

**Se produce en:** paso 2 del flujo principal.  
**Condición:** alguien intenta editar o borrar un registro de la auditoría.

1. El sistema no lo permite.

**Resultado:** el registro se mantiene sin cambios.

---

### 6. Postcondiciones

- POST-1. La consulta no modifica ningún dato.
