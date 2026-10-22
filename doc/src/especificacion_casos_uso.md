# Especificación de casos de uso

Este apartado describe textualmente los casos de uso del sistema de facturación y cobros de Turbine, representados en los diagramas de casos de uso (`casos_uso.pu`).

**Actores principales:** Usuario operativo, Administrador y Superadministrador. El Administrador hereda los casos de uso del Usuario operativo, y el Superadministrador los del Administrador (RB-18).  

**Actores secundarios:** Planificador del sistema, Stripe (simulado), Servicio de mensajería (simulado) y Sistema externo vía API.  

Los clientes de Turbine y la gestoría no son actores, porque no utilizan la aplicación.

---

## CU-01. Iniciar sesión

**Identificador:** CU-01  
**Nombre:** Iniciar sesión  
**Objetivo:** Permitir que un empleado de Turbine acceda a la aplicación con su cuenta corporativa y con los permisos de su rol.  
**Actores principales:** Usuario operativo (y, por herencia, Administrador y Superadministrador).  
**Actores secundarios:** Ninguno.  

**Precondiciones:**

- El usuario tiene una cuenta creada por un administrador.
- La cuenta está activa.

**Flujo principal:**

1. El usuario abre la pantalla de inicio de sesión.
2. El sistema solicita el email y la contraseña.
3. El usuario introduce sus credenciales.
4. El sistema comprueba que el email pertenece al dominio corporativo de Turbine.
5. El sistema verifica las credenciales y que la cuenta está activa.
6. El sistema identifica el rol del usuario y sus clientes asignados.
7. El sistema inicia la sesión y muestra la pantalla de trabajo diario (CU-23).

**Flujos alternativos:**

- A1. Si el usuario ha olvidado la contraseña, puede recuperarla mediante CU-02.

**Excepciones:**

- E1. Si el email no pertenece al dominio de Turbine, el sistema rechaza el acceso.
- E2. Si las credenciales son incorrectas, el sistema muestra un error sin indicar cuál de los dos datos falla y permite volver a intentarlo.
- E3. Si se producen varios intentos fallidos seguidos, el sistema bloquea temporalmente el acceso a esa cuenta.
- E4. Si la cuenta está desactivada, el sistema deniega el acceso.

**Postcondiciones:**

- Si el acceso es correcto, existe una sesión activa con los permisos del rol del usuario.
- La sesión se cerrará automáticamente tras un periodo de inactividad.
- Si falla, no se crea ninguna sesión.

**Requisitos relacionados:** UR-01; FR-01, FR-02, FR-04; NFR-15, NFR-16, NFR-17.

---

## CU-02. Recuperar contraseña

**Identificador:** CU-02  
**Nombre:** Recuperar contraseña  
**Objetivo:** Permitir que un usuario que ha olvidado su contraseña establezca una nueva de forma segura.  
**Actores principales:** Usuario operativo (y, por herencia, Administrador y Superadministrador).  
**Actores secundarios:** Servicio de mensajería.  
**Relación:** Extiende CU-01 (Iniciar sesión) cuando el usuario ha olvidado la contraseña.  

**Precondiciones:**

- El usuario tiene una cuenta activa.

**Flujo principal:**

1. Desde la pantalla de inicio de sesión, el usuario selecciona "He olvidado mi contraseña".
2. El sistema solicita el email corporativo.
3. El usuario introduce su email.
4. El sistema genera un enlace de recuperación con caducidad.
5. El sistema envía el enlace mediante el servicio de mensajería.
6. El usuario abre el enlace e introduce una nueva contraseña.
7. El sistema guarda la nueva contraseña de forma segura e informa del cambio.

**Flujos alternativos:**

- A1. Si el email no corresponde a ninguna cuenta, el sistema muestra el mismo mensaje que en el caso correcto, para no revelar qué cuentas existen.

**Excepciones:**

- E1. Si el enlace ha caducado, el sistema lo rechaza y permite solicitar uno nuevo.
- E2. Si el envío falla, el sistema informa de que no se ha podido enviar el enlace.

**Postcondiciones:**

- La contraseña queda actualizada y el enlace utilizado deja de ser válido.

**Requisitos relacionados:** UR-01; FR-03; NFR-16.

---

## CU-03. Gestionar usuarios y asignaciones

**Identificador:** CU-03  
**Nombre:** Gestionar usuarios y asignaciones  
**Objetivo:** Crear, modificar y desactivar cuentas de usuario, asignarles un rol y decidir qué clientes gestiona cada usuario operativo.  
**Actores principales:** Administrador (y, por herencia, Superadministrador).  
**Actores secundarios:** Ninguno.  

**Precondiciones:**

- El actor ha iniciado sesión (CU-01).

**Flujo principal:**

1. El actor accede a la gestión de usuarios.
2. El sistema muestra los usuarios existentes, su rol y su estado.
3. El actor selecciona crear un usuario o modificar uno existente.
4. El actor introduce el nombre, el email corporativo y el rol.
5. El sistema comprueba que el email pertenece al dominio de Turbine.
6. El actor asigna o desasigna clientes al usuario, si es un usuario operativo.
7. El sistema guarda los cambios y los registra en la auditoría.

**Flujos alternativos:**

- A1. Desactivar un usuario: el actor desactiva la cuenta de alguien que deja Turbine; el sistema cierra sus sesiones activas y conserva el registro de sus acciones.
- A2. Cambiar el responsable de un cliente: el nuevo responsable pasa a recibir los avisos de ese cliente, y el historial sigue indicando quién hizo cada acción anterior.

**Excepciones:**

- E1. Si faltan datos obligatorios o el email no es del dominio de Turbine, el sistema indica qué hay que corregir.
- E2. Si un Administrador intenta asignar el rol de Superadministrador sin permiso, el sistema rechaza la operación.

**Postcondiciones:**

- El usuario queda creado, modificado o desactivado, con su rol y sus clientes asignados.
- El cambio queda registrado en la auditoría.

**Requisitos relacionados:** UR-02; FR-05, FR-06, FR-07, FR-08, FR-74; RB-18.

---

## CU-04. Gestionar clientes y contactos

**Identificador:** CU-04  
**Nombre:** Gestionar clientes y contactos  
**Objetivo:** Registrar, consultar y mantener actualizadas las empresas cliente y sus personas de contacto.  
**Actores principales:** Usuario operativo (y, por herencia, Administrador y Superadministrador).  
**Actores secundarios:** Ninguno.  

**Precondiciones:**

- El actor ha iniciado sesión.
- Para modificar un cliente existente, el usuario operativo debe tenerlo asignado.

**Flujo principal:**

1. El actor selecciona dar de alta un cliente.
2. El sistema solicita el nombre comercial, la razón social, el identificador fiscal, la dirección fiscal, el país, la moneda, el sector y el responsable interno.
3. El actor introduce los datos.
4. El sistema comprueba si ya existe un cliente con el mismo identificador fiscal.
5. El actor añade una o varias personas de contacto, indicando su función (facturación, impagos, etc.).
6. El sistema valida los datos y guarda el cliente.

**Flujos alternativos:**

- A1. Modificar un cliente: el actor abre la ficha, cambia los datos y el sistema guarda los cambios y los registra en la auditoría.
- A2. Marcar un contacto como inactivo: el contacto deja de recibir comunicaciones, pero se conserva el registro de las facturas que se le enviaron.
- A3. Consultar el historial: el sistema muestra la línea temporal del cliente (alta, facturas, pagos, incidencias, cambios de tarifa y bajas).
- A4. Reactivar un cliente dado de baja: el sistema reutiliza su ficha anterior en lugar de crear un duplicado.
- A5. Asociar a la empresa una persona de contacto que ya existe en otra empresa cliente, sin duplicarla.

**Excepciones:**

- E1. Si ya existe un cliente con el mismo identificador fiscal, el sistema avisa y permite abrir la ficha existente.
- E2. Si faltan datos obligatorios, el sistema indica qué campos hay que completar.
- E3. Si se intenta eliminar el único contacto de facturación de un cliente activo, el sistema avisa antes de continuar.
- E4. Si un usuario operativo intenta acceder a un cliente que no tiene asignado, el sistema deniega el acceso.

**Postcondiciones:**

- El cliente y sus contactos quedan registrados o actualizados.
- Ningún cliente se borra físicamente; las bajas son lógicas.

**Requisitos relacionados:** UR-03, UR-10; FR-08 a FR-15, FR-56, FR-82; RB-01, RB-19.

---

## CU-05. Importar clientes desde CSV

**Identificador:** CU-05  
**Nombre:** Importar clientes desde CSV  
**Objetivo:** Dar de alta varios clientes a la vez a partir de un fichero CSV, sin introducir datos erróneos.  
**Actores principales:** Usuario operativo (y, por herencia, Administrador y Superadministrador).  
**Actores secundarios:** Ninguno.  

**Precondiciones:**

- El actor ha iniciado sesión.
- El actor dispone de un fichero CSV con los datos de los clientes.

**Flujo principal:**

1. El actor accede a la importación de clientes y selecciona el fichero.
2. El sistema lee el fichero y muestra sus columnas.
3. El actor relaciona cada columna con el campo correspondiente del sistema.
4. El sistema valida cada fila y muestra una vista previa, señalando las filas con errores y las que tienen un identificador fiscal duplicado.
5. El actor confirma la importación.
6. El sistema importa solo las filas válidas.
7. El sistema guarda y muestra un resumen con las filas creadas, las rechazadas y el motivo.

**Flujos alternativos:**

- A1. Si el actor cancela antes de confirmar, no se importa ningún cliente.

**Excepciones:**

- E1. Si el fichero no tiene un formato válido, el sistema lo rechaza e indica el motivo.
- E2. Si ninguna fila es válida, el sistema no importa nada e informa de los errores.

**Postcondiciones:**

- Los clientes válidos quedan creados, sin duplicar los existentes.
- El resumen de la importación queda registrado.

**Requisitos relacionados:** UR-04; FR-71, FR-72; A-19.

---

## CU-06. Gestionar suscripción (alta y pausa)

**Identificador:** CU-06  
**Nombre:** Gestionar suscripción (alta y pausa)  
**Objetivo:** Registrar la suscripción mensual de un cliente y pausarla temporalmente cuando se acuerde.  
**Actores principales:** Usuario operativo (y, por herencia, Administrador y Superadministrador).  
**Actores secundarios:** Ninguno.  

**Precondiciones:**

- El actor ha iniciado sesión y tiene asignado al cliente.
- El cliente existe en el sistema.

**Flujo principal (alta):**

1. El actor abre la ficha del cliente y selecciona crear la suscripción.
2. El actor indica la fecha de inicio, la tarifa mensual y las condiciones de pago.
3. El sistema comprueba que el cliente tiene todos los datos fiscales obligatorios.
4. El sistema calcula y muestra el importe prorrateado del primer mes (RB-05).
5. El actor revisa el importe y confirma el alta.
6. El sistema guarda la suscripción y la activa.

**Flujos alternativos:**

- A1. Fecha de inicio futura: el cliente queda pendiente de activación y no se generan facturas hasta esa fecha.
- A2. Condiciones de pago particulares: el actor indica un plazo distinto (por ejemplo, 30 días), que prevalece sobre el general.
- A3. Pausar la suscripción: el actor indica el motivo, la fecha de inicio y la fecha prevista de reactivación; durante la pausa no se generan facturas nuevas (RB-15).
- A4. Fin de la pausa: al llegar la fecha prevista, el sistema avisa al responsable para que confirme la reactivación.
- A5. Excepción comercial: el actor registra un descuento temporal, meses gratuitos o un precio pactado; se ejecuta CU-21 (Solicitar aprobación) y la excepción no se aplica hasta que se aprueba.

**Excepciones:**

- E1. Si al cliente le faltan datos fiscales obligatorios, el sistema impide activar la suscripción e indica cuáles faltan.

**Postcondiciones:**

- La suscripción queda activa, pendiente de activación o pausada.
- El planificador la tendrá en cuenta en la generación mensual (CU-09).

**Requisitos relacionados:** UR-05, UR-10, UR-14; FR-16 a FR-20, FR-52, FR-81; RB-02, RB-05, RB-06, RB-13, RB-15, RB-20.

---

## CU-07. Crear factura puntual

**Identificador:** CU-07  
**Nombre:** Crear factura puntual  
**Objetivo:** Preparar una factura por un servicio adicional (instalación, consultoría, etc.) que no forma parte de la suscripción.  
**Actores principales:** Usuario operativo (y, por herencia, Administrador y Superadministrador).  
**Actores secundarios:** Ninguno.  
**Relación:** Incluye CU-08 (Emitir factura).  

**Precondiciones:**

- El actor ha iniciado sesión y tiene asignado al cliente.

**Flujo principal:**

1. El actor selecciona el cliente y la opción de crear una factura puntual.
2. El actor añade una o varias líneas con concepto, cantidad, precio, descuento e impuesto, eligiendo el impuesto entre los configurados en el sistema.
3. El sistema calcula los importes y guarda la factura como borrador.
4. El actor revisa la factura y decide emitirla.
5. Se ejecuta CU-08 (Emitir factura).

**Flujos alternativos:**

- A1. El actor elige un concepto del catálogo y el sistema rellena el precio orientativo y el impuesto, que el actor puede modificar.
- A2. El actor deja la factura en borrador para terminarla más tarde; mientras sea borrador puede modificarla o eliminarla.

**Excepciones:**

- E1. Si los datos de alguna línea no son válidos, el sistema indica qué hay que corregir.

**Postcondiciones:**

- La factura queda guardada como borrador o emitida, si se completa CU-08.

**Requisitos relacionados:** UR-05, UR-06; FR-21, FR-22, FR-29, FR-80; RB-16.

---

## CU-08. Emitir factura

**Identificador:** CU-08  
**Nombre:** Emitir factura  
**Objetivo:** Convertir un borrador en una factura definitiva, con número oficial y PDF, y enviarla al cliente.  
**Actores principales:** Usuario operativo (y, por herencia, Administrador y Superadministrador). También se ejecuta incluido en CU-09 (Generar facturas recurrentes).  
**Actores secundarios:** Ninguno.  
**Relaciones:** Incluye CU-10 (Enviar factura). Puede ser extendido por CU-21 (Solicitar aprobación).  

**Precondiciones:**

- Existe una factura en borrador.
- El cliente tiene al menos un contacto de facturación activo.

**Flujo principal:**

1. El actor solicita emitir la factura.
2. El sistema muestra una vista previa.
3. El sistema comprueba que no falta ningún dato fiscal obligatorio.
4. El sistema comprueba si la factura necesita aprobación (RB-13).
5. El actor confirma la emisión.
6. El sistema asigna el siguiente número único y correlativo de la serie.
7. El sistema genera el PDF con la imagen corporativa de Turbine y lo guarda sin cambios.
8. El sistema marca la factura como emitida y lo registra en la auditoría.
9. Se ejecuta CU-10 (Enviar factura).

**Flujos alternativos:**

- A1. Si la factura tiene un descuento, un importe modificado a mano o un importe superior al umbral configurado, se ejecuta CU-21 (Solicitar aprobación) y la factura queda pendiente; se emitirá cuando se apruebe.

**Excepciones:**

- E1. Si falta algún dato fiscal obligatorio, el sistema impide la emisión e indica cuál falta.
- E2. Si dos usuarios emiten facturas a la vez, el sistema garantiza que no reciben el mismo número.
- E3. Si el actor pulsa dos veces el botón de emitir, el sistema avisa de que la operación ya se está procesando y no crea una factura duplicada.

**Postcondiciones:**

- La factura tiene un número definitivo y un PDF que no se modificará.
- La factura ya no puede editarse ni eliminarse; cualquier corrección se hará con CU-12.

**Requisitos relacionados:** UR-05, UR-06; FR-27 a FR-32, FR-59; RB-10, RB-11, RB-13; NFR-11, NFR-13.

---

## CU-09. Generar facturas recurrentes

**Identificador:** CU-09  
**Nombre:** Generar facturas recurrentes  
**Objetivo:** Generar automáticamente cada mes las facturas de todas las suscripciones activas.  
**Actores principales:** Planificador del sistema.  
**Actores secundarios:** Ninguno.  
**Relación:** Incluye CU-08 (Emitir factura).  

**Precondiciones:**

- Es día 1 de mes, o la fecha de inicio de una nueva suscripción.

**Flujo principal:**

1. El planificador inicia el proceso de generación mensual.
2. El sistema obtiene las suscripciones activas.
3. Para cada suscripción, el sistema crea la factura con la tarifa vigente en esa fecha.
4. Se ejecuta CU-08 (Emitir factura) para cada factura; las recurrentes normales no necesitan aprobación.
5. El sistema registra la ejecución: clientes procesados, facturas creadas y fallos.

**Flujos alternativos:**

- A1. Alta a mitad de mes: en la fecha de inicio de la suscripción se genera una factura proporcional por los días que quedan de mes (RB-05).
- A2. Los clientes pausados, pendientes de activación o dados de baja no generan factura.

**Excepciones:**

- E1. Si falla la factura de un cliente (por ejemplo, porque le falta un dato fiscal), el sistema continúa con el resto y avisa al responsable de ese cliente.
- E2. Si el sistema estaba caído el día 1, al volver genera las facturas pendientes una única vez, sin duplicarlas.

**Postcondiciones:**

- Cada suscripción activa tiene su factura del mes, emitida y enviada.
- La ejecución queda registrada y se puede consultar en CU-27.

**Requisitos relacionados:** UR-05; FR-23 a FR-26, FR-75; RB-04, RB-05, RB-15, RB-20; NFR-11, NFR-12.

---

## CU-10. Enviar factura

**Identificador:** CU-10  
**Nombre:** Enviar factura  
**Objetivo:** Hacer llegar la factura emitida a los contactos de facturación del cliente y registrar el resultado.  
**Actores principales:** Se ejecuta incluido en CU-08 (Emitir factura).  
**Actores secundarios:** Servicio de mensajería.  

**Precondiciones:**

- La factura está emitida y tiene su PDF.

**Flujo principal:**

1. El sistema prepara el mensaje con la plantilla correspondiente, en el idioma del cliente, incluyendo el email y el teléfono de soporte.
2. El sistema envía el mensaje con el PDF por email a los contactos de facturación.
3. El sistema envía también un mensaje por WhatsApp a los contactos que hayan dado su consentimiento.
4. El servicio de mensajería devuelve el resultado del envío.
5. El sistema registra cada envío con su canal, destinatario, fecha y resultado.

**Flujos alternativos:**

- A1. Reenvío: tras corregir un contacto, el actor reenvía la factura; el sistema registra el reenvío como reintento, sin crear una factura nueva ni cambiar su número.

**Excepciones:**

- E1. Si el envío falla o el correo rebota, el sistema no da la factura por notificada, avisa al responsable del cliente y la muestra en la pantalla de trabajo.

**Postcondiciones:**

- Queda registrado a quién se envió la factura, por qué canal y con qué resultado.

**Requisitos relacionados:** UR-07; FR-35 a FR-38, FR-41; NFR-20, NFR-24.

---

## CU-11. Enviar recordatorios

**Identificador:** CU-11  
**Nombre:** Enviar recordatorios  
**Objetivo:** Recordar automáticamente a los clientes las facturas pendientes de pago hasta que se paguen.  
**Actores principales:** Planificador del sistema.  
**Actores secundarios:** Servicio de mensajería.  

**Precondiciones:**

- Existen facturas emitidas y no pagadas.

**Flujo principal:**

1. El planificador inicia diariamente la revisión de facturas pendientes.
2. El sistema calcula, para cada factura, si le corresponde un recordatorio según su vencimiento y las condiciones de pago del cliente (RB-06, RB-07).
3. El sistema prepara el recordatorio con la plantilla correspondiente.
4. El sistema lo envía mediante el servicio de mensajería.
5. El sistema registra el envío y su resultado.
6. Cuando una factura vence sin pagarse, el sistema avisa a su responsable interno.

**Flujos alternativos:**

- A1. Si la factura se paga, el sistema deja de enviar recordatorios.
- A2. Si la factura está en disputa o tiene una incidencia que pausa los recordatorios (CU-15), no se envían hasta la fecha de revisión.
- A3. Si el cliente se ha dado de baja con facturas pendientes, se siguen enviando recordatorios de esas facturas (RB-14).

**Excepciones:**

- E1. Si un envío falla, el sistema lo registra y avisa al responsable.

**Postcondiciones:**

- Los recordatorios enviados y sus resultados quedan registrados.
- Un impago nunca provoca la cancelación del servicio (RB-08).

**Requisitos relacionados:** UR-07, UR-11; FR-39 a FR-41, FR-55, FR-62; RB-06, RB-07, RB-08, RB-14.

---

## CU-12. Crear factura rectificativa

**Identificador:** CU-12  
**Nombre:** Crear factura rectificativa  
**Objetivo:** Corregir una factura ya emitida sin modificarla, mediante una nueva factura vinculada a la original.  
**Actores principales:** Usuario operativo (y, por herencia, Administrador y Superadministrador).  
**Actores secundarios:** Ninguno.  
**Relación:** Incluye CU-21 (Solicitar aprobación).  

**Precondiciones:**

- La factura original está emitida.
- El actor tiene asignado al cliente.

**Flujo principal:**

1. El actor abre la factura emitida y selecciona rectificarla.
2. El sistema muestra los datos de la factura original.
3. El actor indica el motivo y los datos que se corrigen.
4. El sistema prepara la factura rectificativa en la serie de rectificativas.
5. Se ejecuta CU-21 (Solicitar aprobación).
6. Cuando se aprueba, el sistema emite la rectificativa, la vincula con la original y la envía al cliente.

**Flujos alternativos:**

- A1. Si la solicitud se rechaza, la rectificativa no se emite y la factura original no cambia.

**Excepciones:**

- E1. Si el actor intenta editar directamente una factura emitida, el sistema no lo permite e indica que debe usar una rectificativa.

**Postcondiciones:**

- La factura original se conserva intacta.
- La rectificativa queda vinculada a la original y el saldo del cliente se recalcula.

**Requisitos relacionados:** UR-06; FR-29, FR-33, FR-48, FR-59; RB-10, RB-13; A-03.

---

## CU-13. Conciliar pagos

**Identificador:** CU-13  
**Nombre:** Conciliar pagos  
**Objetivo:** Detectar automáticamente los pagos recibidos y asociarlos a sus facturas.  
**Actores principales:** Planificador del sistema.  
**Actores secundarios:** Stripe (simulado).  

**Precondiciones:**

- Existen facturas emitidas pendientes de pago.

**Flujo principal:**

1. El planificador inicia periódicamente la consulta de pagos, o Stripe envía un aviso de pago.
2. El sistema obtiene de Stripe los pagos recibidos.
3. Para cada pago, el sistema busca la factura con la misma referencia e importe.
4. Si la coincidencia es clara, el sistema asocia el pago a la factura y la marca como pagada.
5. El sistema actualiza el saldo del cliente, deja de enviar recordatorios y avisa al responsable interno.

**Flujos alternativos:**

- A1. Si el pago no tiene una referencia clara o puede corresponder a varias facturas, queda pendiente y se ejecuta CU-14 (Asociar pago manualmente).
- A2. Si el importe recibido es menor o mayor que el de la factura, el sistema registra la diferencia, no marca la factura como pagada y la deja pendiente de revisión (CU-14).

**Excepciones:**

- E1. Si llega dos veces el mismo aviso de pago, el sistema lo ignora y no cuenta el pago dos veces.
- E2. Si Stripe no está disponible, el sistema registra el error, no cambia el estado de ninguna factura y lo vuelve a intentar en la siguiente ejecución.

**Postcondiciones:**

- Los pagos claros quedan asociados a sus facturas, y de cada factura pagada se puede consultar el pago que lo justifica.
- Los pagos dudosos quedan pendientes de revisión manual.

**Requisitos relacionados:** UR-08, UR-09; FR-42, FR-43, FR-44, FR-46, FR-48, FR-40, FR-62; RB-12; A-11; NFR-11, NFR-20, NFR-24.

---

## CU-14. Asociar pago manualmente

**Identificador:** CU-14  
**Nombre:** Asociar pago manualmente  
**Objetivo:** Resolver los pagos que el sistema no ha podido asociar automáticamente.  
**Actores principales:** Usuario operativo (y, por herencia, Administrador y Superadministrador).  
**Actores secundarios:** Ninguno.  
**Relación:** Extiende CU-13 (Conciliar pagos) cuando el pago no tiene referencia clara o el importe no coincide.  

**Precondiciones:**

- Existe un pago pendiente de asociar o una diferencia de importe pendiente de revisión.

**Flujo principal:**

1. El actor abre la lista de pagos pendientes desde la pantalla de trabajo.
2. El sistema muestra el pago y las facturas candidatas.
3. El actor selecciona la factura correcta e indica el motivo.
4. El sistema asocia el pago, actualiza el estado de la factura y el saldo del cliente.
5. El sistema registra quién hizo la asociación y por qué.

**Flujos alternativos:**

- A1. Deshacer una asociación incorrecta: el actor la revierte indicando el motivo; el sistema no borra el registro, sino que guarda quién la revirtió y por qué.
- A2. Diferencia de importe: el actor deja constancia de la diferencia; la factura no se marca como pagada hasta que esté cubierta por completo.

**Excepciones:**

- E1. Si el actor no tiene asignado al cliente, el sistema deniega la operación.

**Postcondiciones:**

- El pago queda asociado, o la asociación incorrecta queda revertida, con su registro completo.

**Requisitos relacionados:** UR-09; FR-45, FR-46, FR-47; RB-12; NFR-20.

---

## CU-15. Gestionar disputas e incidencias

**Identificador:** CU-15  
**Nombre:** Gestionar disputas e incidencias  
**Objetivo:** Registrar y seguir los problemas de cobro, pausando los recordatorios cuando sea necesario.  
**Actores principales:** Usuario operativo (y, por herencia, Administrador y Superadministrador).  
**Actores secundarios:** Ninguno.  

**Precondiciones:**

- El actor ha iniciado sesión y tiene asignado al cliente.

**Flujo principal:**

1. El actor abre una incidencia sobre un cliente y, opcionalmente, sobre una factura.
2. El actor indica el tipo (por ejemplo, "cliente reclama importe" o "pago no localizado"), la prioridad y el responsable.
3. El sistema guarda la incidencia como abierta.
4. Los usuarios añaden comentarios internos durante el seguimiento.
5. El actor cierra la incidencia indicando la resolución.

**Flujos alternativos:**

- A1. Disputa: el actor marca la factura en disputa con un motivo y una fecha de revisión; el sistema pausa sus recordatorios, pero la sigue mostrando como no cobrada.
- A2. Si la incidencia afecta al cobro, el actor puede pausar los recordatorios de la factura asociada.
- A3. Al llegar la fecha de revisión de una disputa, el sistema avisa al responsable.

**Excepciones:**

- E1. Si faltan datos obligatorios (tipo, motivo o fecha de revisión), el sistema no guarda la incidencia o la disputa.

**Postcondiciones:**

- La incidencia o disputa queda registrada con su estado, y al cerrarse queda su resolución.

**Requisitos relacionados:** UR-09, UR-11; FR-49, FR-50, FR-62.

---

## CU-16. Declarar factura incobrable

**Identificador:** CU-16  
**Nombre:** Declarar factura incobrable  
**Objetivo:** Dar por imposible de cobrar una factura, de forma controlada y sin marcarla como pagada.  
**Actores principales:** Administrador (y, por herencia, Superadministrador).  
**Actores secundarios:** Ninguno.  
**Relación:** Incluye CU-21 (Solicitar aprobación).  

**Precondiciones:**

- La factura está emitida y no está pagada.

**Flujo principal:**

1. El administrador abre la factura y selecciona declararla incobrable.
2. El administrador indica el motivo.
3. Se ejecuta CU-21 (Solicitar aprobación).
4. Cuando se aprueba, el sistema marca la factura como incobrable, detiene sus recordatorios y lo registra en la auditoría.

**Flujos alternativos:**

- A1. Si se rechaza, la factura sigue pendiente y continúa su proceso de cobro.

**Excepciones:**

- E1. Si no se indica el motivo, el sistema no permite enviar la solicitud.

**Postcondiciones:**

- La factura queda como incobrable, que es un estado distinto de "pagada" (RB-12).

**Requisitos relacionados:** UR-09, UR-14; FR-51, FR-74; RB-12, RB-13.

---

## CU-17. Gestionar baja de cliente

**Identificador:** CU-17  
**Nombre:** Gestionar baja de cliente  
**Objetivo:** Dar de baja la suscripción de un cliente, dejando de facturarle sin perder sus deudas ni su historial.  
**Actores principales:** Usuario operativo (y, por herencia, Administrador y Superadministrador). También se ejecuta incluido en CU-18.  
**Actores secundarios:** Ninguno.  

**Precondiciones:**

- El cliente tiene una suscripción activa o pausada.

**Flujo principal:**

1. El actor registra la solicitud de baja del cliente e indica la fecha efectiva.
2. El sistema comprueba si el cliente tiene facturas pendientes.
3. El sistema muestra al actor las consecuencias de la baja.
4. El actor confirma la baja.
5. En la fecha efectiva, el sistema cierra la suscripción y deja de generar facturas recurrentes.
6. El sistema conserva la ficha y el historial del cliente (baja lógica).

**Flujos alternativos:**

- A1. Si el cliente tiene facturas pendientes, la baja se realiza igualmente, pero el sistema mantiene los recordatorios de esas facturas hasta que se paguen (RB-14).
- A2. Si el actor cancela antes de confirmar, no se realiza ningún cambio.

**Excepciones:**

- E1. Si el actor no tiene asignado al cliente, el sistema deniega la operación.

**Postcondiciones:**

- El cliente no recibe nuevas facturas recurrentes desde la fecha de baja.
- Sus facturas pendientes se siguen reclamando y su historial se conserva.

**Requisitos relacionados:** UR-10; FR-12, FR-53, FR-54, FR-55; RB-14, RB-19, RB-20; Q-01.

---

## CU-18. Comunicar solicitud de baja (API)

**Identificador:** CU-18  
**Nombre:** Comunicar solicitud de baja (API)  
**Objetivo:** Permitir que otro sistema de Turbine comunique que un cliente quiere darse de baja.  
**Actores principales:** Sistema externo vía API.  
**Actores secundarios:** Ninguno.  
**Relación:** Incluye CU-17 (Gestionar baja de cliente).  

**Precondiciones:**

- El sistema externo dispone de credenciales válidas para la API.

**Flujo principal:**

1. El sistema externo envía a la API la solicitud de baja de un cliente.
2. El sistema comprueba la autenticación y los permisos de la llamada.
3. El sistema registra la solicitud y la llamada.
4. Se ejecuta CU-17 (Gestionar baja de cliente), aplicando las reglas de la aplicación.
5. La API devuelve que la solicitud ha sido registrada.

**Flujos alternativos:**

- A1. El sistema crea una tarea para el responsable del cliente, que revisa y confirma la baja.

**Excepciones:**

- E1. Si las credenciales no son válidas, la API rechaza la petición.
- E2. Si el cliente no existe, la API devuelve un error.
- E3. Si se superan los límites de peticiones, la API rechaza las llamadas sobrantes.

**Postcondiciones:**

- La solicitud de baja queda registrada y se gestiona con las mismas reglas que una baja manual.
- El sistema externo no puede cerrar la suscripción directamente.

**Requisitos relacionados:** UR-18; FR-53, FR-77, FR-78; NFR-15, NFR-25.

---

## CU-19. Consultar facturas pendientes (API)

**Identificador:** CU-19  
**Nombre:** Consultar facturas pendientes (API)  
**Objetivo:** Permitir que otro sistema de Turbine sepa si un cliente tiene facturas pendientes de pago.  
**Actores principales:** Sistema externo vía API.  
**Actores secundarios:** Ninguno.  

**Precondiciones:**

- El sistema externo dispone de credenciales válidas para la API.

**Flujo principal:**

1. El sistema externo envía a la API el identificador de un cliente.
2. El sistema comprueba la autenticación y los permisos de la llamada.
3. El sistema calcula si el cliente tiene facturas pendientes.
4. La API devuelve si existen obligaciones pendientes y el detalle mínimo autorizado (número de facturas e importe pendiente).
5. El sistema registra la llamada.

**Flujos alternativos:**

- A1. Si el cliente no tiene facturas pendientes, la API lo indica.

**Excepciones:**

- E1. Si las credenciales no son válidas, la API rechaza la petición.
- E2. Si el cliente no existe, la API devuelve un error.

**Postcondiciones:**

- La consulta no modifica ningún dato.

**Requisitos relacionados:** UR-18; FR-76, FR-78; NFR-15, NFR-25.

---

## CU-20. Revisar tarifa anual

**Identificador:** CU-20  
**Nombre:** Revisar tarifa anual  
**Objetivo:** Revisar una vez al año el rango de tarifa de cada cliente según su facturación anual.  
**Actores principales:** Administrador (y, por herencia, Superadministrador).  
**Actores secundarios:** Ninguno.  
**Relación:** Incluye CU-21 (Solicitar aprobación).  

**Precondiciones:**

- Ha llegado el periodo de revisión anual.
- Los rangos de tarifa están configurados (CU-26).

**Flujo principal:**

1. El sistema muestra la lista de clientes cuyo rango debe revisarse.
2. El administrador selecciona un cliente e introduce su facturación anual, indicando la fuente y la fecha del dato.
3. El sistema propone el rango y el precio que le corresponden.
4. Si el rango cambia, se ejecuta CU-21 (Solicitar aprobación).
5. Cuando se aprueba, el sistema aplica el nuevo precio desde la fecha de efecto y registra el precio anterior, el nuevo y quién lo aprobó.

**Flujos alternativos:**

- A1. Si el rango no cambia, el sistema registra la revisión sin modificar la tarifa.
- A2. Si el dato de facturación no está disponible, la revisión queda pendiente.

**Excepciones:**

- E1. Si se rechaza el cambio, se mantiene la tarifa anterior.

**Postcondiciones:**

- La revisión queda registrada.
- Las facturas anteriores no se modifican y no se generan facturas retroactivas.

**Requisitos relacionados:** UR-15; FR-57, FR-58; RB-02, RB-03; A-09.

---

## CU-21. Solicitar aprobación

**Identificador:** CU-21  
**Nombre:** Solicitar aprobación  
**Objetivo:** Dejar pendiente de aprobación una operación económica sensible hasta que la valide un administrador.  
**Actores principales:** Se ejecuta incluido en CU-12, CU-16 y CU-20, o como extensión de CU-08.  
**Actores secundarios:** Ninguno.  

**Precondiciones:**

- La operación es de las que requieren aprobación (RB-13).

**Flujo principal:**

1. El sistema crea la solicitud con la operación, el solicitante y el motivo.
2. El sistema indica al solicitante por qué la operación queda pendiente.
3. El sistema añade la solicitud a la bandeja de aprobaciones y avisa a los aprobadores.
4. La operación queda en espera hasta que se resuelva mediante CU-22.

**Flujos alternativos:**

- A1. Si el importe supera el segundo umbral configurado, la solicitud debe aprobarla el Superadministrador.

**Excepciones:**

- E1. Si falta el motivo, el sistema no crea la solicitud.

**Postcondiciones:**

- La operación no se aplica hasta que se apruebe.
- El solicitante puede consultar el estado de su solicitud en la bandeja.

**Requisitos relacionados:** UR-14; FR-59, FR-60; RB-13; A-12.

---

## CU-22. Aprobar o rechazar solicitudes

**Identificador:** CU-22  
**Nombre:** Aprobar o rechazar solicitudes  
**Objetivo:** Revisar y resolver las operaciones económicas pendientes de aprobación.  
**Actores principales:** Administrador (y, por herencia, Superadministrador).  
**Actores secundarios:** Ninguno.  

**Precondiciones:**

- Existen solicitudes pendientes en la bandeja de aprobaciones.

**Flujo principal:**

1. El administrador abre la bandeja de aprobaciones.
2. El sistema muestra las solicitudes pendientes con su motivo.
3. El administrador selecciona una solicitud y revisa su detalle.
4. El administrador la aprueba o la rechaza, añadiendo un comentario.
5. El sistema aplica la operación si se aprueba, o la descarta si se rechaza.
6. El sistema registra la decisión en la auditoría y avisa al solicitante.

**Flujos alternativos:**

- A1. El solicitante consulta el resultado y el comentario del aprobador en su bandeja.

**Excepciones:**

- E1. Si el administrador intenta aprobar su propia solicitud y el importe supera el umbral de separación de funciones, el sistema no lo permite.
- E2. Si la solicitud requiere al Superadministrador, un Administrador no puede aprobarla.

**Postcondiciones:**

- La solicitud queda aprobada o rechazada, con el comentario y el aprobador registrados.

**Requisitos relacionados:** UR-14; FR-60, FR-61, FR-62, FR-74; RB-13; A-12.

---

## CU-23. Consultar pantalla de trabajo y notificaciones

**Identificador:** CU-23  
**Nombre:** Consultar pantalla de trabajo y notificaciones  
**Objetivo:** Ver de un vistazo lo que hay que resolver hoy y los avisos de los clientes asignados.  
**Actores principales:** Usuario operativo (y, por herencia, Administrador y Superadministrador).  
**Actores secundarios:** Ninguno.  

**Precondiciones:**

- El actor ha iniciado sesión.

**Flujo principal:**

1. Al entrar, el sistema muestra la pantalla de trabajo diario.
2. La pantalla incluye las facturas que vencen pronto, las vencidas, los pagos pendientes de asociar, los envíos fallidos y las aprobaciones pendientes de los clientes del actor.
3. El sistema muestra también las notificaciones del actor (pagos recibidos, facturas vencidas, disputas, revisiones anuales, aprobaciones resueltas).
4. El actor selecciona un elemento y el sistema le lleva directamente a él para resolverlo.

**Flujos alternativos:**

- A1. El actor marca una notificación como leída o como resuelta.
- A2. Un administrador ve la información de todos los clientes, no solo de los asignados.

**Excepciones:**

- E1. Si no hay elementos pendientes, el sistema lo indica.

**Postcondiciones:**

- La consulta no modifica datos, salvo el estado de las notificaciones.

**Requisitos relacionados:** UR-11; FR-62, FR-63; BR-04.

---

## CU-24. Buscar clientes y facturas

**Identificador:** CU-24  
**Nombre:** Buscar clientes y facturas  
**Objetivo:** Encontrar rápidamente cualquier cliente o factura.  
**Actores principales:** Usuario operativo (y, por herencia, Administrador y Superadministrador).  
**Actores secundarios:** Ninguno.  

**Precondiciones:**

- El actor ha iniciado sesión.

**Flujo principal:**

1. El actor escribe en la búsqueda global parte del nombre, la razón social, el identificador fiscal, una persona de contacto, un email o un número de factura.
2. El sistema muestra los resultados que el actor tiene permiso para ver.
3. El actor selecciona un resultado y el sistema muestra su ficha.

**Flujos alternativos:**

- A1. Filtrar clientes por ubicación, sector y estado.
- A2. Filtrar facturas por estado, tipo (puntual o recurrente), cliente y fecha.
- A3. Si no hay resultados, el sistema lo indica y permite cambiar la búsqueda.
- A4. En el listado de clientes, el sistema muestra el estado de cada uno (activo, pausado, baja solicitada…) sin necesidad de abrir su ficha.

**Excepciones:**

- E1. Si el actor intenta abrir un cliente o factura que no tiene asignado, el sistema deniega el acceso.

**Postcondiciones:**

- La búsqueda no modifica ningún dato.

**Requisitos relacionados:** UR-08, UR-12; FR-08, FR-34, FR-69, FR-70, FR-79; RB-20; NFR-08.

---

## CU-25. Consultar dashboard e informes

**Identificador:** CU-25  
**Nombre:** Consultar dashboard e informes  
**Objetivo:** Conocer la situación financiera de Turbine y exportar informes.  
**Actores principales:** Administrador (y, por herencia, Superadministrador).  
**Actores secundarios:** Ninguno.  

**Precondiciones:**

- El actor ha iniciado sesión con rol de Administrador o Superadministrador.

**Flujo principal:**

1. El administrador abre el dashboard.
2. El sistema muestra lo facturado, cobrado, pendiente y vencido por semana, mes, trimestre y año, el tiempo medio de cobro, los clientes con mayor deuda y la previsión de ingresos recurrentes.
3. El administrador pulsa una cifra y el sistema muestra las facturas que la componen.
4. El administrador genera un informe filtrando por periodo, cliente, responsable, país, sector, estado o tipo de factura.
5. El administrador exporta el informe a CSV o PDF.

**Flujos alternativos:**

- A1. Generar el paquete periódico para la gestoría, con las facturas emitidas, las rectificativas, los cobros y sus PDF.
- A2. Si el informe es muy grande, el sistema lo genera en segundo plano y avisa cuando está listo.

**Excepciones:**

- E1. Si un usuario operativo intenta acceder al dashboard, el sistema deniega el acceso.
- E2. Si no hay datos para el periodo elegido, el sistema lo indica.

**Postcondiciones:**

- La consulta no modifica datos. Las exportaciones incluyen solo los datos que el usuario puede ver.

**Requisitos relacionados:** UR-13; FR-64 a FR-68; RB-21; A-07, A-17; NFR-06, NFR-09, NFR-14.

---

## CU-26. Configurar el sistema

**Identificador:** CU-26  
**Nombre:** Configurar el sistema  
**Objetivo:** Modificar los parámetros de negocio sin tocar el código.  
**Actores principales:** Superadministrador.  
**Actores secundarios:** Ninguno.  

**Precondiciones:**

- El actor ha iniciado sesión como Superadministrador.

**Flujo principal:**

1. El superadministrador abre la configuración.
2. El sistema muestra los parámetros actuales: plazo de pago por defecto, cadencia de recordatorios, dominio de correo, datos fiscales de Turbine, logo, series de facturación, impuestos, rangos de tarifa y umbrales de aprobación.
3. El superadministrador modifica uno o varios parámetros.
4. El sistema valida que los valores son coherentes.
5. El sistema guarda los cambios y los registra en la auditoría.

**Flujos alternativos:**

- A1. Editar las plantillas de mensaje en español e inglés.

**Excepciones:**

- E1. Si algún valor no es válido (por ejemplo, un plazo negativo o una serie duplicada), el sistema no guarda los cambios e indica el error.
- E2. Si un usuario que no es Superadministrador intenta acceder, el sistema deniega el acceso.

**Postcondiciones:**

- La nueva configuración se aplica solo a partir de ese momento; las facturas ya emitidas no cambian (RB-17).

**Requisitos relacionados:** UR-16; FR-41, FR-73, FR-74; RB-17; NFR-26.

---

## CU-27. Consultar auditoría y estado del sistema

**Identificador:** CU-27  
**Nombre:** Consultar auditoría y estado del sistema  
**Objetivo:** Saber quién hizo cada acción y comprobar que los procesos automáticos funcionan.  
**Actores principales:** Superadministrador.  
**Actores secundarios:** Ninguno.  

**Precondiciones:**

- El actor ha iniciado sesión como Superadministrador.

**Flujo principal:**

1. El superadministrador abre la auditoría.
2. El sistema muestra las acciones registradas: quién, qué y cuándo.
3. El superadministrador filtra por usuario, tipo de acción, cliente o fecha.
4. El superadministrador abre la pantalla de estado del sistema.
5. El sistema muestra la última generación de facturas, los mensajes pendientes de enviar, los errores recientes y la última copia de seguridad.

**Flujos alternativos:**

- A1. El superadministrador consulta el detalle de una ejecución automática: elementos procesados, fallos y motivo.

**Excepciones:**

- E1. Si alguien intenta modificar o borrar un registro de auditoría, el sistema no lo permite.

**Postcondiciones:**

- La consulta no modifica ningún dato.

**Requisitos relacionados:** UR-17; FR-74, FR-75; NFR-21, NFR-22.
