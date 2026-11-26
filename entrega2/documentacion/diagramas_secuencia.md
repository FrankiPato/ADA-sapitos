# Diagramas UML de secuencia

## Selección de escenarios

Hemos hecho cuatro diagramas (`diagramas/secuencias/`). Los hemos elegido porque son los casos de uso con más reglas de negocio y en los que surgieron las dudas de la entrevista y del informe del cliente:

| Diagrama | Caso de uso | Por qué lo hemos elegido |
|---|---|---|
| `CU-06.puml` | CU-06 Gestionar suscripción | Alta a mitad de mes con prorrateo, una de las reglas más importantes del sistema (RB-05) |
| `CU-12.puml` | CU-12 Crear factura rectificativa | El cliente nos pidió expresamente ver cómo se genera una rectificativa y cómo queda vinculada a la original |
| `CU-13.puml` | CU-13 Conciliar pagos | Proceso automático con tres casos distintos: pago correcto, importe distinto y pago sin referencia (RB-12) |
| `CU-17.puml` | CU-17 Gestionar baja de cliente | Baja de un cliente con facturas pendientes, que era la principal contradicción de la entrevista (Q-01, RB-14) |

En todos usamos la misma notación que en clase: `actor` para los actores, `boundary` para las pantallas, `control` para los servicios, `entity` para los objetos del dominio y `database` para la base de datos. Los mensajes van numerados y los retornos con línea discontinua. Los mensajes con flecha abierta son asíncronos (por ejemplo, enviar una factura o avisar al responsable).

## Descripción de CU-06 (alta con prorrateo)

1. El usuario introduce la fecha de inicio y la tarifa de la suscripción.
2. `ServicioClientes` comprueba que el cliente tiene todos los datos fiscales. Si falta alguno, se muestra cuál y no se continúa (FE-1).
3. Se crea la suscripción y se calcula el importe prorrateado del primer mes, que se muestra antes de confirmar (FR-17). Por ejemplo, un alta el 14/11 con 890 €/mes son 890 × 17 / 30 = 504,33 €.
4. Si hay una excepción comercial, se pide aprobación (FA-4, RB-13).
5. Si la fecha de inicio es futura, el cliente queda pendiente de activación (FA-1). Si empieza hoy, se activa y se genera la factura proporcional.
6. Se guarda la suscripción en la base de datos y se le da la confirmación al usuario.

## Descripción de CU-12 (factura rectificativa)

1. El usuario pide rectificar una factura emitida e indica el motivo.
2. `ServicioFacturacion` crea la rectificativa y pide la aprobación, porque las rectificativas siempre la necesitan (RB-13).
3. Cuando el administrador la aprueba, se emite con un número de la serie, la original pasa a estado RECTIFICADA y la rectificativa se envía al cliente.
4. Si se rechaza, la rectificativa no se emite y la original no cambia (FE-1).
5. La factura original nunca se modifica (RB-10).

## Descripción de CU-13 (conciliar pagos)

1. El planificador lanza cada día la consulta de pagos a Stripe (simulado).
2. Si Stripe no responde, se registra el error y no se cambia ninguna factura (FE-2).
3. Para cada pago, se comprueba primero que no esté ya registrado, para no contarlo dos veces (FE-1).
4. Después se busca la factura por su referencia, y hay tres casos:
   - **Misma referencia e importe:** se asocia y la factura se marca como pagada.
   - **Importe distinto:** se asocia, pero la factura no se marca como pagada y queda la diferencia pendiente de revisar (FA-3, RB-12).
   - **Sin referencia clara:** el pago queda pendiente de asociar a mano en CU-14 (FA-2).
5. En los tres casos se avisa al responsable del cliente.

## Descripción de CU-17 (baja con facturas pendientes)

1. El usuario pide dar de baja al cliente con una fecha efectiva.
2. Se comprueba que el usuario tiene asignado al cliente (FE-1).
3. Se buscan sus facturas pendientes y se avisa de que se seguirán reclamando (FA-1).
4. Si el usuario confirma, el cliente queda en "baja solicitada". Si cancela, no cambia nada (FA-2).
5. En la fecha efectiva, el planificador procesa la baja y cierra la suscripción. Desde ese momento no se generan más facturas, pero las pendientes se siguen reclamando (RB-14, Q-01).

## Trazabilidad

| Diagrama | Caso de uso | Participantes / responsabilidades |
|---|---|---|
| `CU-06.puml` | CU-06 (FR-16 a FR-20, FR-81; RB-05, RB-13) | PantallaSuscripcion, ServicioClientes, Cliente, Suscripcion, ServicioAprobaciones, ServicioFacturacion, BaseDatos |
| `CU-12.puml` | CU-12 y CU-22 (FR-33, FR-59; RB-10, RB-11, RB-13) | PantallaFacturas, ServicioFacturacion, ServicioAprobaciones, Factura, FacturaRectificativa, Serie, ServicioEnvios, BaseDatos |
| `CU-13.puml` | CU-13 (FR-42 a FR-46, FR-62; RB-12) | Planificador, ServicioCobros, Stripe (simulado), Factura, ServicioEnvios, BaseDatos |
| `CU-17.puml` | CU-17 (FR-53 a FR-55; RB-14, RB-19) | PantallaCliente, ServicioClientes, UsuarioOperativo, Cliente, Baja, Suscripcion, Planificador, BaseDatos |

## Consistencia

- **Con el diagrama de clases:** los servicios (`control`) y los objetos del dominio (`entity`) son clases del diagrama de clases, y los mensajes son sus operaciones (por ejemplo, `prepararAlta`, `calcularProrrateo`, `crearRectificativa`, `marcarRectificada`, `registrarDiferencia` o `hacerEfectiva`).
- **Diferencias de nivel de abstracción:** las pantallas (`PantallaSuscripcion`, `PantallaFacturas`, `PantallaCliente`) y `BaseDatos` no aparecen en el diagrama de clases. Las pantallas forman parte de la capa de presentación y `BaseDatos` representa la capa de persistencia, y las dos aparecen en el diagrama de arquitectura. Algunos mensajes a `BaseDatos` (`guardar`, `obtenerBajas`…) representan consultas, no operaciones de una clase.
- **Con los casos de uso:** el actor "Planificador del sistema" es el mismo que en los casos de uso; dentro del sistema lo recibe la clase `Planificador`. Stripe aparece como sistema externo, igual que en los casos de uso y en la arquitectura.
- **Con el modelo de datos:** lo que se guarda en `BaseDatos` corresponde a las tablas SUSCRIPCION, FACTURA, PAGO, CONCILIACION, CLIENTE y BAJA.
