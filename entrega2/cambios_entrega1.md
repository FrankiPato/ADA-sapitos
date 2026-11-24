# Cambios respecto a la Entrega 1

Los cambios salen de dos revisiones: el **informe de evaluación del profesor** de la Entrega 1 y el **informe de Alejandro Ruiz** (cliente) tras la presentación del 09/11. Las fuentes actualizadas están en `entrega1-actualizada/fuentes/`.

## Cambios ya hechos

| Elemento | Cambio | Motivo | Impacto en Entrega 2 | Validación / pendiente |
|---|---|---|---|---|
| Requisitos, apartado 8.3 (nuevo) | Tabla de trazabilidad de cada regla de negocio con sus requisitos funcionales y casos de uso | Informe del profesor: las reglas (prorrateo, cobros, alta) solo aparecían dentro de los flujos | Las decisiones de clases, ER y secuencias se pueden justificar con su regla (por ejemplo, RB-05 en `Suscripcion.calcularProrrateo` y en la secuencia CU-06) | Hecho |
| Requisitos, apartado 8.4 (nuevo) | Tabla de trazabilidad de cada requisito funcional con su caso de uso | Informe del profesor: faltaba enlazar FR con CU | Base para la trazabilidad de los `.md` de documentación | Hecho. Ningún FR queda sin caso de uso |
| Requisitos, apartado 8.5 | El antiguo 8.3 (ejemplo de la baja con deuda) pasa a ser el 8.5 y se actualiza su frase final | Reorganización por las tablas nuevas | Ninguno | Hecho |
| Requisitos, apartado 1.4 (nuevo) | Tabla de procesos automáticos: quién los inicia, qué hace el sistema solo y cuándo interviene una persona | Informe del profesor: revisar los límites de la automatización | Define las tareas del `Planificador` en clases, secuencias (CU-13, CU-17) y arquitectura | Hecho |
| Requisitos 1.3, A-18 y glosario | El "Planificador del sistema" se define como temporizador (el paso del tiempo), no como componente | Informe del profesor: separar actor externo, temporizador y componente interno | El temporizador es un actor; dentro del sistema lo recibe el componente `Planificador` (clases y arquitectura) | Hecho |
| Requisitos 1.2 | Se quita el texto de la plantilla que se había quedado | Corrección | Ninguno | Hecho |
| UR-19 y A-21 | UR-19 (sustituciones temporales) se añade a las tablas 8.1 y 8.2, y se quita la "delegación temporal" de A-21 | Había una contradicción: A-21 la aplazaba, pero UR-19 y FR-07 la incluían | `AsignacionCliente` con tipo HABITUAL o SUSTITUCION y fechas (clases y ER) | Hecho |
| FR-83 (nuevo) | Pausar los recordatorios cuando el cliente llama para negociar un aplazamiento, con fecha de reanudación | Informe del cliente: pidió poder pausar los avisos tras una llamada comercial | `Incidencia` de tipo APLAZAMIENTO con `fechaReanudacion`; `ServicioCobros.registrarAplazamiento` (clases y ER) | Hecho |
| RB-14 | Se deja claro que la baja no cancela ni condona las deudas | Informe del cliente: pidió que la regla lo dijera de forma explícita | Secuencia CU-17 (baja con facturas pendientes) | Hecho |
| RB-22 (nueva) | Mientras los recordatorios están pausados, la factura sigue pendiente y su vencimiento no cambia | Informe del cliente: pregunta sobre cómo se reflejan las pausas en CU-11 | `Factura.recordatoriosPausados` (clases y ER) | Hecho |
| RB-23 (nueva) | El sistema no envía dos veces el mismo mensaje aunque el proceso se repita | Informe del cliente: pregunta sobre cómo se evitan las notificaciones duplicadas | Restricción UNIQUE en la tabla ENVIO y atributo `Envio.fechaPrevista` | Hecho |
| CU-15 | Nuevo flujo alternativo FA-3 "Aplazamiento negociado por teléfono" | Recoger FR-83 en los casos de uso | Operación `registrarAplazamiento` | Hecho |
| CU-11 | FA-2 incluye también los aplazamientos y la pausa dura "hasta que termine la pausa" | Coherencia con FR-83 y RB-22 | Ninguno | Hecho |
| CU-10, CU-11, CU-15 | Se añaden FR-83, RB-22 y RB-23 a sus requisitos relacionados | Coherencia con la trazabilidad | Ninguno | Hecho |
| Trazabilidad 8.1 a 8.4 | Se añaden FR-83, RB-22 y RB-23 | Mantener la trazabilidad completa | Ninguno | Hecho |
| Repositorio | Las fuentes de la Entrega 1 pasan de `doc/src/` a `entrega1-actualizada/fuentes/` | Nueva estructura de carpetas del profesor | Ninguno | Hecho |

## Respuestas a las preguntas del cliente

| Pregunta | Dónde se responde |
|---|---|
| ¿Cómo se reflejan las pausas de los avisos en el CU-11? | CU-11 (FA-2), CU-15 (FA-3) y RB-22 |
| ¿Cuál es la secuencia exacta para generar una factura rectificativa? | Diagrama de secuencia `CU-12.puml` de esta entrega |
| ¿Cómo se evitan las notificaciones duplicadas? | RB-23 y la restricción UNIQUE de la tabla ENVIO |

## Pendiente de confirmar con el cliente

Estas decisiones se mantienen como provisionales. No cambian el diseño, pero conviene confirmarlas:

| Elemento | Hipótesis actual | Pendiente |
|---|---|---|
| Q-01 | Un cliente con deuda puede darse de baja, pero se le sigue reclamando lo pendiente | Confirmar con el cliente (ya lo confirmó de forma implícita en su informe) |
| Q-17 | El plazo de pago de 5 días se cuenta en días naturales | Confirmar con el cliente |
| Q-10 | Los rangos de tarifa son configurables | Pedir la tabla de tarifas actual |
| Q-11, Q-13 | Tiempo de conservación de datos y datos fiscales obligatorios de clientes extranjeros | Confirmar con la gestoría |
| Q-12 | Campos del PDF de la factura | Recibir la factura de ejemplo |
| Q-14 | Umbrales de aprobación: 2.000 € y 10.000 € | Confirmar los importes |
