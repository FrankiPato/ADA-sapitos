# Arquitectura y componentes

## Vista representada

El diagrama (`diagramas/arquitectura.puml`) muestra la **arquitectura lógica en capas** del sistema, no su despliegue en servidores. Hemos elegido una arquitectura en capas sobre un modelo cliente-servidor: los usuarios usan la aplicación desde el navegador y toda la lógica está en el servidor.

Hay cinco capas, y cada una solo usa la que tiene debajo:

1. **Presentación:** la interfaz web que usan los empleados y la API REST que usan otros sistemas de Turbine.
2. **Negocio:** el planificador y los servicios con las reglas de negocio. Son las clases de control del diagrama de clases.
3. **Integración:** los adaptadores que se comunican con Stripe y con el servicio de mensajería (email y WhatsApp), que están simulados.
4. **Persistencia:** los repositorios, que son los únicos que acceden a la base de datos.
5. **Datos:** la base de datos (el modelo físico de datos) y el almacén donde se guardan los PDF de las facturas.

## Responsabilidades

| Componente | Responsabilidad | Relaciones principales |
|---|---|---|
| Interfaz web | Pantallas para los usuarios internos, que se adaptan al móvil y a la tablet | Capa de negocio |
| API REST | Punto de entrada para otros sistemas de Turbine, con autenticación por token | ServicioFacturacion, ServicioClientes |
| Planificador | Recibe la señal del temporizador y lanza las tareas del día: facturas del día 1, consulta de pagos, recordatorios, bajas y pausas | ServicioFacturacion, ServicioCobros, ServicioClientes |
| ServicioClientes | Clientes, contactos, importación CSV, suscripciones, pausas y bajas | ServicioAprobaciones, Repositorios |
| ServicioFacturacion | Facturas puntuales, recurrentes, proporcionales y rectificativas, y sus PDF | ServicioAprobaciones, ServicioEnvios, Repositorios |
| ServicioCobros | Consulta de pagos, conciliación, recordatorios, incidencias y aplazamientos | Adaptador de pagos, ServicioEnvios, Repositorios |
| ServicioAprobaciones | Solicitudes de aprobación y su resolución | Repositorios |
| ServicioEnvios | Envío de facturas y recordatorios, y avisos internos a los responsables | Adaptador de mensajería |
| Adaptador de pagos | Pide a Stripe (simulado) los pagos recibidos | Stripe (simulado) |
| Adaptador de mensajería | Envía los mensajes por email o WhatsApp (simulados) y recoge si han llegado | Servicio de mensajería (simulado) |
| Repositorios | Guardan y consultan los datos | Base de datos, almacén de PDF |

## Justificación y alternativas

- **Por qué en capas.** Es una aplicación interna, la hace un equipo pequeño y las operaciones son económicas, así que necesitamos que sea fácil de entender, de probar y de mantener (NFR-26, NFR-27). Separar la interfaz, la lógica y el acceso a datos permite cambiar una parte sin tocar las demás.
- **Separación entre temporizador y componente interno.** El "Planificador del sistema" de los casos de uso es un actor que representa el paso del tiempo. Dentro del sistema, quien recibe esa señal y lanza las tareas es el componente `Planificador`. Así queda clara la diferencia que nos pidió el profesor entre actor externo, temporizador y componente interno (A-18).
- **Adaptadores para los sistemas externos.** Stripe y la mensajería solo se usan a través de su adaptador. Si se cambia Stripe simulado por el real, o se añade otra forma de pago, solo hay que cambiar el adaptador (NFR-24).
- **La API usa los mismos servicios que la interfaz web**, así que aplica las mismas reglas y permisos (FR-78).
- **Restricciones:**
  - La presentación nunca accede directamente a la base de datos.
  - Las tareas pesadas, como generar todas las facturas del mes, se ejecutan en segundo plano para no bloquear la aplicación (NFR-09).
  - Todo el software es libre (NFR-28).
- **Alternativas descartadas:**
  - **Microservicios:** demasiado complejos para un equipo pequeño y una aplicación interna.
  - **Arquitectura orientada a eventos:** los procesos automáticos dependen de fechas, no de eventos en tiempo real, así que basta con un planificador.

## Trazabilidad

| Caso de uso | Componentes que colaboran |
|---|---|
| CU-01, CU-02, CU-03 (acceso y usuarios) | Interfaz web, Repositorios |
| CU-04, CU-05, CU-06, CU-17, CU-20 (clientes y suscripciones) | Interfaz web, ServicioClientes, ServicioAprobaciones, Repositorios |
| CU-07, CU-08, CU-12, CU-16 (facturas) | Interfaz web, ServicioFacturacion, ServicioAprobaciones, ServicioEnvios, Repositorios |
| CU-09 (facturas del día 1) | Planificador, ServicioFacturacion, ServicioEnvios, Repositorios |
| CU-10, CU-11 (envíos y recordatorios) | ServicioEnvios, ServicioCobros, Adaptador de mensajería |
| CU-13, CU-14, CU-15 (cobros e incidencias) | Planificador, ServicioCobros, Adaptador de pagos, Repositorios |
| CU-18, CU-19 (API) | API REST, ServicioClientes, ServicioFacturacion, Repositorios |
| CU-21, CU-22 (aprobaciones) | Interfaz web, ServicioAprobaciones, Repositorios |
| CU-23 a CU-27 (consultas, informes, configuración y auditoría) | Interfaz web, Repositorios |

**Relación con los demás diagramas:**
- Los componentes de la capa de negocio son las clases de control del diagrama de clases.
- Las pantallas de los diagramas de secuencia forman parte de la interfaz web.
- La `BaseDatos` de las secuencias representa los repositorios y la base de datos de este diagrama, cuyas tablas son las del modelo de datos.
