# RutaExpress — Microservicio Shipments (EP2)

Microservicio principal de gestión y seguimiento de envíos del proyecto académico RutaExpress, desarrollado con Java 21 y Spring Boot.

## Función principal

Shipments permite gestionar los envíos registrados en RutaExpress, almacenar su información y coordinar procesos asíncronos mediante RabbitMQ.

## Tecnologías

- Java 21
- Spring Boot
- Spring Data JPA
- Oracle Database
- RabbitMQ
- Maven
- Docker

## Arquitectura

El frontend Angular accede al microservicio mediante AWS API Gateway y el BFF.

**Flujo principal:**

Angular → API Gateway → BFF → Shipments → RabbitMQ → Consumidores

Shipments utiliza Oracle para persistir los envíos.

## Gestión de envíos

El microservicio permite registrar envíos, generar códigos de seguimiento y administrar su estado.

Los estados utilizados incluyen:

- CREADO
- ACEPTADO
- EN_BODEGA
- EN_RUTA
- ENTREGADO
- CANCELADO

## Integración con RabbitMQ

Shipments publica comandos que son procesados por otros microservicios.

| Routing key | Función |
|---|---|
| `email.send` | Solicitar una notificación |
| `label.gen` | Generar una etiqueta |
| `warehouse.ticket` | Registrar un ticket de bodega |

Los consumidores procesan estos mensajes y confirman su recepción mediante ACK.

Los errores no recuperables pueden enviarse a las Dead Letter Queues (DLQ), según la configuración de RabbitMQ.

## Base de datos

Motor: Oracle Database.

Esquema utilizado: `RUTAEXPRESS_SHIPMENTS`.

La conexión se configura mediante propiedades de Spring Boot y variables de entorno. Las credenciales no deben almacenarse en GitHub.

## Compilación

Desde la carpeta del proyecto, en Windows:

```powershell
.\mvnw.cmd -DskipTests clean package
```

El archivo JAR se genera en `target/`.

La opción `-DskipTests` omite la ejecución de pruebas automatizadas.

## Docker

El repositorio incluye un `Dockerfile` para construir la imagen del microservicio.

Puerto interno: **8080**.

El despliegue utilizado en AWS se administra desde el repositorio principal de RutaExpress, mediante `infra/apps/compose.yml`.

## Pruebas realizadas

Durante la EP2 se comprobó:

- Creación de envíos desde Angular.
- Actualización del estado a ACEPTADO.
- Persistencia de envíos en Oracle.
- Solicitud de publicación de eventos RabbitMQ.
- Procesamiento posterior por EMAIL, LABEL y WAREHOUSE.
- Registro de tickets de bodega en Oracle.

## Repositorio principal

https://github.com/isaacduoc/rutaexpress

Proyecto académico RutaExpress — Entrega Parcial 2.