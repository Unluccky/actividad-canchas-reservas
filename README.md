# Sistema de Gestión de Club Deportivo (Microservicios)

Este repositorio contiene una solución basada en **Microservicios** para la gestión de canchas y reservas de un club deportivo, desarrollada con **Java 21**, **Spring Boot 3.2.5** y **Spring Cloud OpenFeign**.

## 🚀 Arquitectura del Proyecto

El sistema se divide en dos microservicios que se comunican de forma síncrona:

1.  **Servicio de Canchas (Port: 8081):** Gestiona el catálogo de instalaciones (Fútbol, Tenis, etc.).
2.  **Servicio de Reservas (Port: 8080):** Gestiona las reservas de los clientes. Utiliza **OpenFeign** para validar que una cancha existe antes de permitir la reserva.



---

## 🛠️ Tecnologías Utilizadas

* **Java 21**
* **Spring Boot 3.2.5**
* **Spring Cloud OpenFeign** (Comunicación entre servicios)
* **Spring Data JPA** (Persistencia de datos)
* **MySQL** (Base de datos)
* **Maven** (Gestión de dependencias)

---

## 📋 Implementación de OpenFeign

Para lograr la integración entre servicios, se realizaron los siguientes pasos:

1.  **Dependencias:** Se añadió `spring-cloud-starter-openfeign` en el `pom.xml` del servicio de Reservas.
2.  **Habilitación:** Se utilizó la anotación `@EnableFeignClients` en la clase principal.
3.  **Cliente Declarativo:** Se creó la interfaz `CanchaCliente` apuntando a `http://localhost:8081`.
4.  **Validación Logica:** En `ReservaServicesImpl`, se implementó un bloque `try-catch` que consulta al servicio de canchas. Si el servicio de canchas devuelve `null` o el ID no existe, se lanza una excepción personalizada.

---

## 🚥 Endpoints Principales

### Microservicio Canchas (8081)
| Método | Endpoint | Descripción |
| :--- | :--- | :--- |
| POST | `/api/canchas` | Registra una nueva cancha. |
| GET | `/api/canchas/{id}` | Obtiene detalles de una cancha específica. |

### Microservicio Reservas (8080)
| Método | Endpoint | Descripción |
| :--- | :--- | :--- |
| POST | `/api/reservas` | Crea una reserva (Valida ID de cancha vía Feign). |
| GET | `/api/reservas/{id}` | Busca una reserva por su ID único. |
| GET | `/api/reservas/cancha/{id}` | Lista todas las reservas de una cancha. |

---

## 🧪 Pruebas de Funcionamiento (Postman)

### Caso Exitoso
Al enviar un `POST` a `/api/reservas` con un `canchaId` existente (ej. ID 3), el sistema devuelve la reserva creada con estado **200 OK**.

### Caso de Error (Validación)
Si se intenta reservar una cancha con un ID inexistente, el servicio de Reservas responde:
> *"Error al validar la cancha con el servicio externo: La cancha con ID XXX no existe."*

---

## ⚙️ Instrucciones de Ejecución
1. Clonar el repositorio.
2. Configurar la base de datos MySQL en los archivos `application.properties` de cada servicio.
3. Ejecutar primero **Servicio Canchas** (8081).
4. Ejecutar segundo **Servicio Reservas** (8080).
