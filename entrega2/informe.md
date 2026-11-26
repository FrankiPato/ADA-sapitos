# Entrega 2 · Diseño del sistema

**Asignatura:** Análisis y Diseño de Aplicaciones · Grupo A · 2026–27  
**Equipo:** [Sapitos]  
**Integrantes:** [Francisco Jiménez Tirado][ Emilio José Aragón Ortegosa] [Marco Gallego Zafra][Ismael García Zamudio][Miguel Vargas Peña][Francisco Sánchez Gámez][Iván González Doña]
**Fecha:** [27/11/2026]  
**Repositorio:** [https://github.com/FrankiPato/ADA-sapitos.git]

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


En este proyecto hemos diseñado la arquitectura completa para un sistema de gestión de clientes, suscripciones y facturación periódica. A lo largo del trabajo, combinando los diagramas de clases (vistas de negocio, facturación y servicios de control) con el modelo relacional físico, hemos buscado dar respuesta a los requerimientos que plantea un modelo de negocio basado en ingresos recurrentes.

El diseño del sistema se articula en tres bloques principales:

1. **Gestión de usuarios y clientes:** Define la estructura de permisos mediante roles (`UsuarioOperativo`, `Administrador`, `Superadministrador`) y contempla el ciclo de vida completo del cliente y sus suscripciones (incluyendo casos reales como pausas temporales, altas, bajas, prorrateos y excepciones en las tarifas).

2. **Facturación, cobros y notificaciones:** Garantiza la correcta emisión de comprobantes (diferenciando facturas normales, proporcionales y rectificativas), la conciliación de los pagos recibidos y la gestión de incidencias, integrando además el envío de recordatorios automáticos por Email o WhatsApp.

3. **Servicios de control y base de datos:** Incorpora una capa de servicios desacoplada y un `Planificador` de tareas programadas que automatiza los procesos diarios. Todo esto se apoya en un modelo físico relacional normalizado que asegura la consistencia de los datos.

### Aspectos Clave y Decisiones de Diseño

Del desarrollo de este trabajo y del análisis de los diagramas se desprenden varias conclusiones técnicas importantes:

* **Control financiero y trazabilidad:** Para evitar inconsistencias o problemas contables, las facturas emitidas no se modifican directamente, sino que los errores o ajustes se corrigen mediante facturas rectificativas o incidencias. Además, la inclusión de la clase `RegistroAuditoria` y de reglas como la aprobación cruzada (que impide que un usuario apruebe sus propias solicitudes si superan cierto importe) aporta seguridad al sistema.

* **Reducción de carga manual mediante automatización:** Al delegar la lógica repetitiva a servicios específicos (`ServicioFacturacion`, `ServicioCobros`, `ServicioEnvios`) y al `Planificador`, se consigue que el sistema genere facturas periódicas y envíe notificaciones sin necesidad de intervención manual constante, reduciendo el riesgo de errores humanos.

* **Flexibilidad sin perder rigor en los datos:** El diseño contempla situaciones complejas habituales en la gestión comercial B2B (descuentos, cambios tarifarios o suspensiones de servicio) manteniendo la precisión numérica (`DECIMAL`) y las restricciones necesarias a nivel de base de datos para evitar pérdidas de información.

### Valoración Final

El trabajo realizado demuestra que la solución planteada no solo cumple con los requisitos funcionales iniciales, sino que además está estructurada siguiendo buenas prácticas de ingeniería del software. La correspondencia directa entre los diagramas UML de clases, el modelo relacional físico, el diagrama de arquitectura y componentes principales y el modelo secuencial de alguno de los casos de uso mas relevantes, garantiza que la propuesta sea coherente, robusta y esté lista para ser llevada a una fase de desarrollo e implementación real.
