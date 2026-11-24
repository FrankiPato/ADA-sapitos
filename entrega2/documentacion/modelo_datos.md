# Modelo físico de datos

## Alcance

El modelo recoge las tablas de la base de datos del sistema de facturación y cobros de Turbine. Sale de las clases del dominio y de registro del diagrama de clases: cada una de esas clases es una tabla, y las relaciones muchos a muchos se resuelven con tablas intermedias. Las clases de control (`Planificador` y los servicios) no guardan datos, así que no tienen tabla.

Son 29 tablas. Para que se lean bien las hemos dividido en tres diagramas (`diagramas/modelo_datos.puml`). Cuando una tabla aparece en un diagrama solo con su clave primaria es porque está completa en otro.

| Diagrama | Tablas |
|---|---|
| 1. Usuarios, clientes y suscripciones | USUARIO, CLIENTE, ASIGNACION_CLIENTE, CONTACTO, CLIENTE_CONTACTO, RANGO_TARIFA, SUSCRIPCION, PAUSA, BAJA, SOLICITUD_APROBACION, REVISION_TARIFA, EXCEPCION_COMERCIAL |
| 2. Facturación, cobros y comunicaciones | FACTURA, LINEA_FACTURA, SERIE, CONCEPTO, IMPUESTO, PAGO, CONCILIACION, PLANTILLA_MENSAJE, ENVIO, INCIDENCIA, INCIDENCIA_FACTURA, COMENTARIO |
| 3. Control y registro del sistema | NOTIFICACION, REGISTRO_AUDITORIA, EJECUCION_PROCESO, PARAMETRO, IMPORTACION_CSV |

En los diagramas, `*` indica que la columna es obligatoria (NOT NULL).

## Justificación del modelo

- **Herencias en una sola tabla.** Los tres roles de usuario se guardan en `USUARIO` con la columna `rol`, y los cuatro tipos de factura en `FACTURA` con la columna `tipo`. Las columnas que solo usan algunos tipos (periodo, días facturados, factura rectificada, motivo de la rectificación) pueden quedar vacías. Lo hemos preferido a una tabla por tipo porque casi todas las columnas son comunes y así las consultas de facturas son más sencillas.
- **Tablas intermedias.** `ASIGNACION_CLIENTE` (usuario–cliente, con fechas para las sustituciones), `CLIENTE_CONTACTO` (cliente–contacto, con la función del contacto), `CONCILIACION` (pago–factura, con el importe aplicado) e `INCIDENCIA_FACTURA` (una incidencia o aplazamiento puede afectar a varias facturas).
- **Relaciones principales:**
  - `CLIENTE (1)` — `SUSCRIPCION (0..N)`: un cliente puede tener varias suscripciones a lo largo del tiempo, por ejemplo si se da de baja y se reactiva.
  - `CLIENTE (1)` — `FACTURA (0..N)` y `SUSCRIPCION (0..1)` — `FACTURA (0..N)`: las facturas puntuales no tienen suscripción.
  - `FACTURA (1)` — `LINEA_FACTURA (1..N)`: toda factura tiene al menos una línea.
  - `FACTURA (0..1)` — `FACTURA (0..N)`: una rectificativa apunta a la factura que corrige.
  - `PAGO` y `FACTURA` se relacionan muchos a muchos a través de `CONCILIACION`.
  - `USUARIO (1)` — `SOLICITUD_APROBACION (0..N)` como solicitante, y `USUARIO (0..1)` como aprobador, porque mientras está pendiente no hay aprobador.
- **Datos fiscales copiados.** `FACTURA.datos_fiscales_emitidos` guarda los datos del cliente en el momento de emitir, para que una factura emitida no cambie aunque se modifique el cliente (RB-10, FR-32).
- **No hay borrado físico.** Clientes, contactos y usuarios se desactivan o anonimizan con las columnas `estado`, `activo` y `anonimizado` (RB-19, FR-12).

## Reglas y restricciones

La notación del diagrama no muestra todas las restricciones, así que las recogemos aquí:

| Restricción | Regla |
|---|---|
| `USUARIO.email` y `CLIENTE.id_fiscal` únicos | FR-11 |
| `UNIQUE (FACTURA.id_serie, FACTURA.numero)`: número único dentro de cada serie | RB-11 |
| `UNIQUE (FACTURA.id_suscripcion, FACTURA.periodo_desde)`: una sola factura recurrente por suscripción y mes | NFR-11 |
| `PAGO.referencia_externa` única: un pago de Stripe no se registra dos veces | FR-44 |
| `UNIQUE (ENVIO.id_factura, id_contacto, canal, tipo, fecha_prevista)`: no se envía dos veces el mismo mensaje | RB-23 |
| `UNIQUE (EJECUCION_PROCESO.proceso, periodo)`: un proceso automático no se ejecuta dos veces para el mismo periodo | NFR-11 |
| `FACTURA.estado` solo admite los estados de RB-09, y `CLIENTE.estado` los de RB-20 | RB-09, RB-20 |
| `FACTURA.tipo` ∈ {PUNTUAL, RECURRENTE, PROPORCIONAL, RECTIFICATIVA}; `USUARIO.rol` ∈ {OPERATIVO, ADMINISTRADOR, SUPERADMINISTRADOR} | RB-18 |
| `INCIDENCIA.tipo` ∈ {DISPUTA, INCIDENCIA, APLAZAMIENTO}; si es APLAZAMIENTO, `fecha_reanudacion` es obligatoria | FR-83, RB-22 |
| `LINEA_FACTURA.cantidad > 0` y `descuento` entre 0 y 100 | FR-21 |
| Importes con `DECIMAL` y 2 decimales, nunca con decimales en coma flotante | RB-16, NFR-14 |
| `SOLICITUD_APROBACION.id_aprobador` distinto de `id_solicitante` si el importe supera el umbral (se controla en la aplicación) | FR-61 |
| `REGISTRO_AUDITORIA` solo admite inserciones: no se modifican ni se borran filas | NFR-21 |
| Una factura emitida no se modifica; solo cambian su `estado` y `recordatorios_pausados` | RB-10 |

## Trazabilidad

| Requisitos/casos | Tablas/columnas | Observación |
|---|---|---|
| FR-01 a FR-08, RB-18 · CU-01, CU-02, CU-03 | USUARIO, ASIGNACION_CLIENTE | Usuarios, roles y clientes asignados (también las sustituciones, UR-19) |
| FR-09 a FR-15, FR-82, RB-01, RB-19, RB-20 · CU-04, CU-24 | CLIENTE, CONTACTO, CLIENTE_CONTACTO | Datos de los clientes y sus contactos |
| FR-71, FR-72 · CU-05 | IMPORTACION_CSV | Resumen de cada importación |
| FR-16 a FR-20, FR-52 a FR-56, RB-05, RB-14, RB-15 · CU-06, CU-17, CU-18 | SUSCRIPCION, PAUSA, BAJA | Alta con prorrateo, pausas y bajas |
| FR-57, FR-58, FR-81, RB-02, RB-03 · CU-20 | RANGO_TARIFA, REVISION_TARIFA, EXCEPCION_COMERCIAL | Tarifas, revisión anual y excepciones |
| FR-21 a FR-33, FR-80, RB-04, RB-09 a RB-11, RB-16 · CU-07, CU-08, CU-09, CU-12 | FACTURA, LINEA_FACTURA, SERIE, CONCEPTO, IMPUESTO | Facturas de todos los tipos, con sus líneas y numeración |
| FR-42 a FR-48, RB-12 · CU-13, CU-14 | PAGO, CONCILIACION | Pagos recibidos y su asociación a facturas |
| FR-35 a FR-41, RB-07, RB-23 · CU-10, CU-11 | ENVIO, PLANTILLA_MENSAJE | Registro de cada envío y su resultado |
| FR-49, FR-50, FR-83, RB-22 · CU-15 | INCIDENCIA, INCIDENCIA_FACTURA, COMENTARIO, FACTURA.recordatorios_pausados | Disputas, incidencias y aplazamientos |
| FR-51, FR-59 a FR-61, RB-13 · CU-16, CU-21, CU-22 | SOLICITUD_APROBACION | Solicitudes y su resolución |
| FR-62, FR-63 · CU-23 | NOTIFICACION | Avisos internos |
| FR-73 a FR-75, RB-17, NFR-21 · CU-26, CU-27 | PARAMETRO, REGISTRO_AUDITORIA, EJECUCION_PROCESO | Configuración, auditoría y estado de los procesos |

## Decisiones abiertas

- **Tiempo de conservación de los datos** (Q-11): lo debe confirmar la gestoría. Se guardará como un registro de `PARAMETRO`.
- **Datos fiscales obligatorios para clientes de otros países** (Q-13): de momento solo son obligatorias las columnas de razón social, identificador fiscal y dirección. Si la gestoría pide más, se añadirán columnas a `CLIENTE`.
- **Tabla de rangos de tarifa** (Q-10): las filas de `RANGO_TARIFA` se cargarán cuando Turbine nos pase su tabla actual.
- **Alternativa descartada:** una tabla por cada tipo de factura. La descartamos porque casi todas las columnas eran comunes y habría complicado las consultas y los informes.
