# Documentacion de arquitectura



## Repositorio Raíz

Contiene los archivos de configuración de construcción y despliegue del proyecto.

* **Archivos de Build & Deploy:**
    * `pom.xml` / `build.gradle` (Configuración de dependencias y build)
    * `mvnw` (Wrapper de Maven para ejecutar la aplicación)
    * `Dockerfile` (Instrucciones para crear la imagen de contenedor)
    * `compose.yaml` (Definición de servicios de Docker Compose)
* **Código Fuente Principal:** Se encuentra en la carpeta `src`.

---

## Código Principal (`src/main/java`)

Esta carpeta contiene la lógica central de la aplicación, organizada por capas:

| Carpeta/Paquete | Responsabilidad Principal | Archivos de Ejemplo |
| :--- | :--- | :--- |
| **`com.example.rawsource`** | **Paquete Raíz:** Punto de entrada y configuración general de la aplicación. | `RawsourceApplication.java` (`@SpringBootApplication`). |
| **`config`** | Configuración de infraestructura, *beans* personalizados, seguridad, CORS y filtros. | `SecurityConfig.java`, `JwtAuthenticationFilter.java`, `RateLimitFilter.java`. |
| **`controllers`** | **Capa de Presentación:** Controladores REST que definen los *endpoints* de la API. | `UserController.java`, `ProductController.java`. |
| **`entities`** | **Capa de Modelo:** Clases que representan entidades del dominio y *enums*. | `User.java`, `Product.java`. |
| **`entities.dto`** | **Data Transfer Objects (DTOs):** Modelos de datos para la comunicación en *requests* y *responses*. | `entities.dto.user` (subpaquete), `UserDto.java`. |
| **`services`** | **Capa de Negocio:** Lógica de negocio e interfaces de servicio. | `UserService.java`. Subpaquete `impl` para implementaciones concretas. |
| **`repositories`** | **Capa de Persistencia:** Interfaces de Spring Data JPA para la interacción con la base de datos. | `UserRepository.java`, `ProductRepository.java`. |
| **`exceptions`** | Clases para errores personalizados y manejo de excepciones. | `ResourceNotFoundException.java`. |
| **`security`** | Utilidades y servicios relacionados con JWT, autenticación y autorización. | `JwtTokenProvider.java`, `JwtAuthentication.java`. |
| **`utils`** | Clases auxiliares (*helpers*) y utilidades varias. | `DateUtils.java`. |

---

## Recursos (`src/main/resources`)

Archivos de configuración del entorno y *logging*:

* `application.properties` / `application-railway.properties`: Configuración de perfiles, base de datos, JWT, etc.
* `logback-spring.xml`: Configuración del *logging*.

---

## Pruebas (`src/test/java`)

* **Estructura:** Las pruebas (unitarias y de integración) siguen la **misma estructura de paquetes** que el código fuente.
* **Ejemplo de Test:** `UserControllerTest.java`
* **Anotaciones Comunes:** Los tests de controladores usan `@WebMvcTest`, `MockMvc`, y `@WithMockUser` para aislar la prueba de la lógica de negocio.
