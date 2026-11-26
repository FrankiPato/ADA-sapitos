# Entrega 2 · Diseño del sistema

**Asignatura:** Análisis y Diseño de Aplicaciones · Grupo A · 2026–27  
**Equipo:** [nombre del equipo]  
**Integrantes:** [nombres y apellidos]  
**Fecha:** [dd/mm/aaaa]  
**Repositorio:** [URL]

> **Plantilla de índice para componer el PDF manualmente.** Sustituid el texto de ejemplo por vuestro trabajo e incorporad manualmente los diagramas renderizados y el contenido de los archivos de documentación. El archivo Markdown no se transforma automáticamente en PDF.

## 1. Introducción y alcance

Ejemplo ficticio: «Este informe presenta el diseño de un sistema de gestión de pedidos a partir de RF-01 y CU-01». Sustituidlo por el alcance real de vuestro proyecto.

## 2. Actualizaciones de la Entrega 1

Estas modificaciones ya están aplicadas en `cambios_entrega1.md` y tienen como objetivo reforzar la trazabilidad, delimitar la automatización del sistema, corregir contradicciones internas y cubrir nuevas necesidades de gestión de cobros y bajas.

### Requisitos, reglas de negocio y supuestos modificados

* **FR-83 (Nuevo):** Permite pausar los recordatorios de pago con una fecha de reanudación cuando el cliente llama para negociar un aplazamiento.
  * *Motivo:* Petición directa del cliente tras una llamada comercial.
* **RB-22 (Nueva):** Establece que, mientras los recordatorios estén pausados, la factura sigue constando como pendiente y su fecha de vencimiento no varía.
  * *Motivo:* Aclarar la duda del cliente sobre cómo se reflejan estas pausas en el flujo de cobros.
* **RB-23 (Nueva):** Garantiza que el sistema no envíe dos veces el mismo mensaje aunque se repita el proceso de ejecución (implementado con una restricción `UNIQUE` en la tabla `ENVIO`).
  * *Motivo:* Responder a la pregunta del cliente sobre cómo evitar notificaciones duplicadas.
* **RB-14 (Modificada):** Deja explícito que la baja de un cliente no cancela ni condona sus deudas anteriores.
  * *Motivo:* El cliente solicitó que la regla lo indicara de forma expresa.
* **UR-19 y A-21 (Modificados):** Se incorpora UR-19 (sustituciones temporales) a las tablas de trazabilidad 8.1 y 8.2 y se elimina la «delegación temporal» de la asunción A-21.
  * *Motivo:* Resolver una contradicción interna, ya que A-21 la aplazaba mientras que UR-19 y FR-07 ya la incluían.
* **A-18, Glosario y apartado 1.3 (Modificados):** El «Planificador del sistema» pasa a definirse como un actor externo de tipo temporizador (paso del tiempo) y no como un componente interno.
  * *Motivo:* Indicación del profesor para separar actores externos, temporizadores y componentes internos (como la clase `Planificador`).

### Casos de uso modificados

* **CU-15 (Modificado):** Incorpora el nuevo flujo alternativo **FA-3 («Aplazamiento negociado por teléfono»)**, vinculado a la operación `registrarAplazamiento`.
  * *Motivo:* Recoger el nuevo requisito FR-83 dentro de los casos de uso.
* **CU-11 (Modificado):** Amplía el flujo alternativo **FA-2** para incluir también los aplazamientos y precisar que la suspensión dura «hasta que termine la pausa».
  * *Motivo:* Mantener la coherencia con FR-83 y RB-22.
* **CU-10, CU-11 y CU-15 (Trazabilidad):** Se añaden FR-83, RB-22 y RB-23 a sus respectivas secciones de requisitos relacionados.
  * *Motivo:* Asegurar la coherencia y completitud de la trazabilidad.

### Otros cambios estructurales y documentación

* **Nuevas tablas de trazabilidad y automatización:** A petición del profesor, se crean los apartados **8.3** y **8.4** (para enlazar reglas de negocio con requisitos funcionales y casos de uso, justificando así el diseño de clases, ER y secuencias) y el apartado **1.4** (tabla de procesos automáticos que delimita qué hace el sistema solo y cuándo interviene una persona). Como consecuencia, el antiguo apartado 8.3 pasa a ser el **8.5**.
* **Limpieza y estructura de carpetas:** Se elimina texto residual de plantilla en el apartado **1.2**, se reubican las fuentes de `doc/` a `entrega1-actualizada/fuentes/` y se responde a la duda del cliente sobre las facturas rectificativas mediante el diagrama `CU-12.puml`.

### Decisiones pendientes de confirmar con el cliente

Se mantienen seis hipótesis provisionales que no alteran el diseño actual, pero requieren validación:

* **Q-01:** Reclamar la deuda pendiente tras la baja del cliente (confirmado de forma implícita en su informe).
* **Q-17:** Cómputo del plazo de pago de 5 días en días naturales.
* **Q-10:** Rangos de tarifa configurables (pendiente recibir la tabla de tarifas actual).
* **Q-11 y Q-13:** Tiempo de conservación de datos y requisitos fiscales obligatorios de clientes extranjeros (a confirmar con la gestoría).
* **Q-12:** Campos exactos del PDF de la factura (pendiente recibir factura de ejemplo).
* **Q-14:** Umbrales de aprobación de 2.000 € y 10.000 €.

## 3. Modelo físico de datos (Entidad–Relación)

Insertad aquí el diagrama exportado de `diagramas/modelo_datos.puml` y la explicación de `documentacion/modelo_datos.md`. Incluid relaciones, PK/FK y decisiones relevantes.

## 4. Diagrama UML de clases

Insertad el diagrama exportado de `diagramas/diagrama_clases.puml` y la explicación de `documentacion/diagrama_clases.md`.

## 5. Diagramas UML de secuencia

Insertad los diagramas exportados de `diagramas/secuencias/` y la explicación de `documentacion/diagramas_secuencia.md`. Identificad el caso de uso y el escenario que cubre cada uno.

## 6. Arquitectura y componentes

Insertad el diagrama exportado de `diagramas/arquitectura.puml` y la explicación de `documentacion/arquitectura.md`.

## 7. Informe Scrum

Incorporad el contenido cumplimentado de `informe_scrum.md`, junto con las capturas reales del tablero y las evidencias exigidas.

## 8. Conclusiones y referencias (si proceden)

Sintetizad las decisiones más importantes y las cuestiones pendientes. Revisad que los nombres e identificadores sean consistentes en todo el documento.
