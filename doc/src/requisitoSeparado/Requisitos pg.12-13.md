# ***Requisitos pg.12-13***



### **Requisitos de negocios:**

1\. Los centros de coste o proyectos son solo clasificación para informes (ampliación opcional); renombrar una categoría no destruye el histórico.

2\. Los objetivos de facturación y cobro son informativos, los gestionan solo administradores y nunca alteran facturas. Se conserva el objetivo vigente o constancia de cada cambio.

3\. Las reglas corporativas las cambian el administrador o el superadministrador según impacto. El usuario solo puede cambiar preferencias personales y pausar un caso concreto, con permiso y motivo.

4\. Las reglas corporativas las cambian el administrador o el superadministrador según impacto. El usuario solo puede cambiar preferencias personales y pausar un caso concreto, con permiso y motivo.

5\. Una factura conserva su moneda original. El importe convertido para reporting va aparte y no altera el total legal (conversión como ampliación).

6\. Los estados del cliente que afectan a facturación son: pendiente de activación, activo, pausado, baja solicitada, inactivo y quizá bloqueado. Los prospectos (CRM) quedan fuera, y cada cambio de estado tiene consecuencias explícitas.

7\. No se activa recurrencia sin datos fiscales suficientes.

8\. La baja con facturas pendientes se permite, pero se mantiene el recobro.

9\. Se avisa antes de eliminar el único contacto de facturación de un cliente activo.

10\. En operaciones sensibles, el aprobador no aprueba su propia solicitud. Los umbrales y las operaciones afectadas son configurables.

11\. Si el aprobador no está, un administrador reasigna o actúa un suplente, y todo queda registrado.

12\. Los periodos pueden cerrarse (lo hace un administrador) y reabrirse (permiso superior y motivo). Cerrado, no se admiten cambios económicos ordinarios, solo un procedimiento excepcional auditado.

13\. Se entrega a la gestoría un paquete periódico de emitidas, rectificativas, cobros y PDFs. La gestoría no tiene usuario en la v1 y un futuro rol externo sería limitado y temporal.

14\. Los datos de facturación no se usan para marketing. Se guarda el consentimiento de canal por contacto.

15\. Los comentarios tienen dos niveles: normal y restringido a administradores.

16\. Las exportaciones masivas deben poder detectarse.

17\. Los archivos temporales (informes y exportaciones) y los documentos contables tienen políticas de conservación distintas.

18\. No se afirma lo que no se puede saber, por ejemplo que un mensaje fue “leído”.



### **Requisitos de usuarios:**

##### Administrador

**1.** Configurar objetivos y alertas parametrizables.

2\. Administrar calendario laboral y categorías de proyecto.

3\. Reasignar aprobaciones.

4\. Cerrar periodos.

5\. Exportar y regenerar el paquete de gestoría.

6\. Ver notas restringidas.

7\. Detectar exportaciones masivas.

##### Superadministrador

8\. Reabrir periodos cerrados indicando un motivo.

##### General

9\. Cambiar el estado de un cliente viendo antes las consecuencias.

10\. Recibir explicaciones claras cuando algo se bloquea.

11\. Consultar su bandeja de aprobaciones (estado y comentario) y aprobar o rechazar.

12\. Seguir tareas largas mientras sigue trabajando.

13\. Ver la cola de emails y WhatsApp simulados y sus estados.

14\. Escribir comentarios normales o restringidos.

15\. Fijar sus preferencias personales, como el resumen por email.



### **Requisitos de sistema:**

#### **Requisitos funcionales:**

1\. Generar notificaciones cuando se cumplan las alertas configuradas.

2\. Mostrar el progreso respecto al objetivo.

3\. Guardar importe original, importe convertido y tipo de cambio por separado.

4\. Bloquear la activación de recurrencia y explicar qué falta.

5\. Impedir la autoaprobación cuando proceda.

6\. Ejecutar tareas largas en segundo plano (como generar 3.000 PDFs) con estado persistente: pendiente, procesando, completado con errores.

7\. Evitar duplicados por doble clic en emisión, envío y conciliación, avisando con “ya se está procesando”.

8\. Bloquear cambios económicos en periodos cerrados salvo procedimiento excepcional.

9\. Generar el paquete de gestoría, registrando cuándo se creó, qué periodo cubre y su versión.

10\. Cargar datos de demo sin mezclarlos con producción.

11\. Gestionar la cola de mensajes con sus estados: en cola, enviado, entregado si es posible, fallido y reintentado.

12\. Incluir en las exportaciones fecha y hora, filtros y usuario, y auditar las masivas.

13\. Conservar los informes y exportaciones un tiempo configurable y permitir regenerarlos.

#### **Requisitos no funcionales:**

1\. Aritmética decimal exacta, sin coma flotante. Redondeo documentado. PDF, base de datos y dashboard muestran las mismas cifras.

2\. Umbrales, plazos y calendario no están en código.

3\. Las transiciones de estado son deterministas y sin efectos ocultos. Las tareas largas sobreviven al cierre de pestaña. Las operaciones críticas son idempotentes.

4\. Quedan registradas las reasignaciones de aprobación, los cierres y reaperturas de periodo y las exportaciones.

5\. Los permisos por rol cubren paquetes de gestoría, exportaciones y notas restringidas, y el modelo admite un rol externo mínimo y temporal.

6\. Los datos de demo se aíslan de producción. Se aplica minimización y separación de finalidad respecto al marketing.

7\. El modelo admite multimoneda, centros de coste y nuevos canales sin rehacerse.

8\. Los mensajes de bloqueo y estado son comprensibles. La interfaz no se congela en tareas pesadas.

