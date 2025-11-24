

## Guía para agregar nuevas funciones (Feature Development)

**Propósito:** orientar a nuevos desarrolladores sobre el flujo recomendado para añadir un endpoint/funcionalidad completa respetando arquitectura, pruebas y buenas prácticas.

- **Paso 1 — Crea una rama feature:**
    - **Comando:** `git checkout -b feature/CP-xxx-descripcion-corta`

- **Paso 2 — Añade/actualiza la entidad (si aplica):**
    - Si la nueva función necesita persistencia, crea o actualiza la entidad en `src/main/java/com/example/rawsource/entities`.
    - Añade campos necesarios y marca con anotaciones JPA (`@Entity`, `@Table`, `@Id`, etc.).

- **Paso 3 — Repositorio:**
    - Crea una interfaz en `src/main/java/com/example/rawsource/repositories` que extienda `JpaRepository`/`CrudRepository`.
    - Define consultas personalizadas con `@Query` o métodos por nombre cuando sea necesario.

- **Paso 4 — DTOs y mapeo:**
    - Añade DTOs en `src/main/java/com/example/rawsource/entities/dto` (por ejemplo `entities.dto.product.ProductDto`).
    - Usa constructores o librerías (MapStruct / manual) para mapear entre entidad y DTO.

- **Paso 5 — Servicio (lógica de negocio):**
    - Declara la interfaz en `src/main/java/com/example/rawsource/services` (ej. `ProductService`).
    - Implementa la lógica en `src/main/java/com/example/rawsource/services/impl/ProductServiceImpl`.
    - Mantén la lógica de negocio desacoplada del controlador.

- **Paso 6 — Controlador (API):**
    - Añade el endpoint en `src/main/java/com/example/rawsource/controllers` (ej. `ProductController`).
    - Anota rutas con `@RestController` y `@RequestMapping`, y los métodos con `@GetMapping`, `@PostMapping`, etc.
    - Valida entradas con `@Valid` y `jakarta.validation` cuando corresponda.

- **Paso 7 — Seguridad y autorización:**
    - Actualiza `SecurityConfig` (`src/main/java/com/example/rawsource/config/SecurityConfig.java`) si necesitas nuevas reglas de acceso.
    - Para pruebas unitarias de controladores, usa `@WithMockUser` o deshabilita filtros con `@AutoConfigureMockMvc(addFilters = false)`.

- **Paso 8 — Tests:**
    - **Unit tests (servicio):** añade pruebas en `src/test/java/.../services` usando `@ExtendWith(MockitoExtension.class)` y `@Mock` para dependencias.
    - **Controller tests:** crea `@WebMvcTest` y prueba endpoints con `MockMvc` (mocar servicios con `@MockBean`).
    - **Integration tests (opcional):** usa `@SpringBootTest` para pruebas end-to-end, con base de datos embebida si es necesario.
    - Ejecuta tests localmente: 
        ```bash
        ./mvnw -Dtest=YourTestClass test
        ./mvnw test
        ```

- **Paso 9 — Logging y manejo de errores:**
    - Lanza excepciones específicas desde la capa de servicio y conviértelas en respuestas HTTP en `@ControllerAdvice`.
    - Usa el logger (`org.slf4j.Logger`) en clases clave para trazabilidad.

- **Paso 10 — Documentación y API:**
    - Actualiza documentación (README / `DocumentacionTecnica.md`) con la nueva ruta, parámetros y ejemplos.
    - Si usas Swagger/OpenAPI, añade las anotaciones correspondientes para exponer el endpoint.

- **Paso 11 — Calidad de código y formato:**
    - Sigue las convenciones del proyecto (uso de Lombok, nombres de paquetes, formateo).
    - Ejecuta formatter/linter si está configurado, o configura el IDE (IntelliJ/Eclipse) con el estilo del proyecto.

- **Paso 12 — Commit y PR:**
    - Commit claros: `git add . && git commit -m "CP-xxx: Add product creation endpoint"`
    - Incluye tests y documentación en el PR.
    - PR checklist mínima:
        - [ ] Compila (`./mvnw -DskipTests=false package`).
        - [ ] Tests unitarios pasan.
        - [ ] Nuevas dependencias justificadas en `pom.xml`.
        - [ ] Documentación actualizada.

## Buenas prácticas y consejos

- **Separación de responsabilidades:** Controlador no debe contener lógica de negocio compleja; delega al servicio.
- **Pruebas:** Prioriza pruebas unitarias en servicios y pruebas de controlador para la capa web.
- **Seguridad:** Mantén las reglas en `SecurityConfig` y evita hardcodear roles en controladores (usa `@PreAuthorize`).
- **Manejo de excepciones:** Usa `@ControllerAdvice` para mapear excepciones a respuestas HTTP homogéneas.
- **Revisión de dependencias:** Antes de añadir una dependencia al `pom.xml`, evalúa impacto y licencia.
