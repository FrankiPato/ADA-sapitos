# Diagrama UML de clases

## Alcance

El diagrama representa la estructura estática del sistema de facturación y cobros de Turbine: las clases del dominio (clientes, suscripciones, facturas, pagos…) y las clases de control que aplican las reglas de negocio. Los sistemas externos (Stripe y el servicio de mensajería simulados) no se modelan como clases: aparecen en el diagrama de arquitectura.

Tiene 40 clases (29 del dominio, 5 de registro del sistema y 6 de control). Como no se leen bien en una sola página, lo hemos dividido en tres vistas detalladas (`diagramas/diagrama_clases.puml`):

| Vista | Contenido |
|---|---|
| Vista 1 | Usuarios, clientes, contactos, suscripciones, pausas, bajas y tarifas |
| Vista 2 | Facturas, líneas, series, impuestos, pagos, conciliaciones, envíos e incidencias |
| Vista 3 | Clases de control (planificador y servicios) y registro del sistema (notificaciones, auditoría, procesos, parámetros e importaciones) |

Cuando una clase aparece en una vista sin atributos es porque está detallada en otra; se muestra en su respectivo paquete y se pone solo para mostrar la relación.

## Decisiones de diseño

- **Herencia de roles.** `Administrador` hereda de `UsuarioOperativo` y `Superadministrador` de `Administrador`. Es la misma generalización que en los diagramas de casos de uso: cada rol puede hacer todo lo del anterior (RB-18, A-01).
- **`Factura` es abstracta** y tiene cuatro subclases: puntual, recurrente, proporcional y rectificativa. Las cuatro comparten casi todo, pero cada una tiene datos propios (periodo, días facturados, factura que rectifica).
- **La factura guarda una copia de los datos fiscales** del cliente al emitirse, porque una factura emitida no puede cambiar aunque después cambien los datos del cliente o la plantilla (RB-10, FR-32).
- **Composiciones.** `Factura`–`LineaFactura`, `Factura`–`Envio`, `Cliente`–`Suscripcion`, `Suscripcion`–`Pausa`, `Suscripcion`–`ExcepcionComercial` e `Incidencia`–`Comentario`: las partes no tienen sentido sin el todo.
- **Clases asociación.** `AsignacionCliente` (fechas de una asignación o sustitución, FR-07 y UR-19), `ClienteContacto` (función de un contacto en cada empresa, FR-82) y `Conciliacion` (importe aplicado de un pago a una factura, FR-43 y FR-47).
- **`Pago` y `Factura` se relacionan muchos a muchos** a través de `Conciliacion`: un pago puede cubrir varias facturas y una factura puede pagarse en varios pagos. Solo se marca pagada cuando el total está cubierto (RB-12).
- **Seis clases de control** agrupan las reglas de negocio por área: `ServicioClientes` (clientes, suscripciones, pausas y bajas), `ServicioFacturacion`, `ServicioCobros` (pagos, recordatorios e incidencias), `ServicioAprobaciones`, `ServicioEnvios` y `Planificador`. Hemos preferido pocas clases con responsabilidades claras antes que una por cada caso de uso.
- **`Planificador`** es la parte interna del sistema que ejecuta lo que en los casos de uso es el actor "Planificador del sistema": cada día lanza las tareas de facturación, cobros, bajas y pausas. Se relaciona con esos servicios mediante agregación (A-18).
- **Los sistemas externos no son clases.** Stripe y la mensajería se usan desde `ServicioCobros` y `ServicioEnvios`, y se representan en el diagrama de arquitectura (A-08, NFR-24).
- **`EjecucionProceso` con proceso y periodo únicos**, para que si el proceso del día 1 se lanza dos veces no se dupliquen facturas (NFR-11).
- **`Envio` con factura, tipo y fecha prevista únicos**, para no enviar dos veces el mismo mensaje aunque el Servicio de mensajería tarde en responder (RB-23).
- **Aplazamientos negociados.** Se representan como una `Incidencia` de tipo `APLAZAMIENTO` con `fechaReanudacion`, que pausa los recordatorios de las facturas afectadas hasta esa fecha (FR-83, RB-22).
- **Las operaciones** representan las responsabilidades de cada clase, no las firmas finales de la implementación.

### Enumeraciones

Para no cargar el diagrama, las enumeraciones se indican como tipo de los atributos y sus valores se recogen aquí:

| Enumeración | Valores |
|---|---|
| Rol | OPERATIVO, ADMINISTRADOR, SUPERADMINISTRADOR |
| EstadoCliente | PENDIENTE_ACTIVACION, ACTIVO, PAUSADO, BAJA_SOLICITADA, INACTIVO (RB-20) |
| EstadoSuscripcion | PENDIENTE, ACTIVA, PAUSADA, FINALIZADA |
| EstadoFactura | BORRADOR, PENDIENTE_APROBACION, EMITIDA, ENVIADA, ENVIO_FALLIDO, VENCIDA, PAGADA, EN_DISPUTA, DIFERENCIA_IMPORTE, RECTIFICADA, ANULADA, INCOBRABLE (RB-09) |
| EstadoPago | ASOCIADO, PENDIENTE_ASOCIAR, CON_DIFERENCIA |
| TipoIncidencia | DISPUTA, INCIDENCIA, APLAZAMIENTO |
| TipoSolicitud | DESCUENTO, IMPORTE_ALTO, CAMBIO_IMPORTE, RECTIFICATIVA, INCOBRABLE, CAMBIO_TARIFA, EXCEPCION |
| TipoProceso | FACTURACION_MENSUAL, RECORDATORIOS, CONCILIACION, BAJAS_Y_PAUSAS, COPIA_SEGURIDAD |
| Otras | TipoAsignacion, FuncionContacto, Idioma, OrigenBaja, TipoExcepcion, TipoSerie, TipoConciliacion, EstadoIncidencia, TipoEnvio, Canal, ResultadoEnvio, TipoPlantilla, EstadoSolicitud, TipoNotificacion |

## Trazabilidad

| Requisito / CU | Clases y operaciones | Motivo |
|---|---|---|
| FR-01 a FR-08, RB-18 · CU-01, CU-02, CU-03 | `UsuarioOperativo.iniciarSesion`, `recuperarContrasena`, `Administrador`, `Superadministrador`, `AsignacionCliente` | Acceso, roles y permisos por cliente asignado |
| FR-09 a FR-15, FR-71, FR-72, FR-82, RB-01, RB-19, RB-20 · CU-04, CU-05, CU-24 | `Cliente`, `Contacto`, `ClienteContacto`, `ServicioClientes.importarCSV`, `ImportacionCSV` | Gestión de clientes, contactos e importación |
| FR-16 a FR-20, FR-52 a FR-56, RB-05, RB-14, RB-15 · CU-06, CU-17, CU-18 | `Suscripcion.calcularProrrateo`, `Pausa`, `Baja`, `ServicioClientes` | Alta con prorrateo, pausas y bajas |
| FR-57, FR-58, FR-81, RB-02, RB-03 · CU-20 | `RangoTarifa`, `RevisionTarifa`, `ExcepcionComercial` | Tarifas y su revisión anual |
| FR-21 a FR-34, FR-80, RB-04, RB-09 a RB-11, RB-16 · CU-07, CU-08, CU-09, CU-12 | `Factura` y subclases, `LineaFactura`, `Serie`, `Impuesto`, `ServicioFacturacion` | Facturas puntuales, recurrentes, proporcionales y rectificativas |
| FR-42 a FR-48, RB-12 · CU-13, CU-14 | `Pago`, `Conciliacion`, `ServicioCobros.consultarPagos`, `asociarPago` | Consulta de pagos y conciliación |
| FR-35 a FR-41, RB-07, RB-08, RB-23 · CU-10, CU-11 | `Envio`, `PlantillaMensaje`, `ServicioEnvios`, `ServicioCobros.enviarRecordatoriosDelDia` | Envío de facturas y recordatorios sin duplicados |
| FR-49, FR-50, FR-83, RB-22 · CU-15 | `Incidencia`, `Comentario`, `ServicioCobros.registrarAplazamiento` | Disputas, incidencias y aplazamientos |
| FR-51, FR-58 a FR-61, RB-13 · CU-16, CU-21, CU-22 | `SolicitudAprobacion`, `ServicioAprobaciones` | Aprobaciones con separación de funciones |
| FR-62 a FR-68 · CU-23, CU-25 | `Notificacion`, `ServicioEnvios.notificarResponsable`, `Administrador.consultarDashboard`, `generarInforme` | Avisos, dashboard e informes |
| FR-73 a FR-75, RB-17, NFR-21 · CU-26, CU-27 | `Parametro`, `Superadministrador.modificarParametro`, `RegistroAuditoria`, `EjecucionProceso` | Configuración, auditoría y estado de los procesos |
| FR-23, FR-39, FR-42, A-18 · CU-09, CU-11, CU-13 | `Planificador`, `EjecucionProceso` | Procesos que se inician por fecha u hora |

## Consistencia con otros modelos

- **Modelo de datos.** Las clases del dominio (vistas 1 y 2) corresponden a tablas del ER con el mismo nombre. No todas las clases son tablas: las seis clases de control no guardan datos. La herencia de `Factura` se guarda en una sola tabla `FACTURA` con una columna `tipo`, y la de los roles en la tabla `USUARIO` con una columna `rol`. Las clases asociación pasan a ser tablas intermedias.
- **Secuencias.** Los objetos de los diagramas de secuencia son instancias de estas clases, y los mensajes son sus operaciones.
- **Arquitectura.** Cada clase de control corresponde a un componente de la capa de negocio. La comunicación con Stripe y con el servicio de mensajería se representa allí, en la capa de integración.
