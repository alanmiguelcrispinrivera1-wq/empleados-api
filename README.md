# Empleados API

API REST para administrar empleados, construida paso a paso en la Academia Java CDMX
(Semana 3, del 24 al 26 de septiembre de 2026).

**Alumno:** Alan Miguel Crispin Rivera

## Tecnologías

Java 17 · Spring Boot 4.1.1 · Spring Data JPA (Hibernate) · Bean Validation · MySQL 8.4 en Docker ·
springdoc-openapi (Swagger) · WSL2 con Ubuntu 24.04

## Cómo levantarla

```bash
docker compose up -d            # MySQL en Docker; espera a que docker compose ps diga (healthy)
./mvnw spring-boot:run          # la API en http://localhost:8080
```

- Swagger: http://localhost:8080/swagger-ui.html
- Datos de ejemplo (30 empleados):
  `docker exec -i empleados-mysql mysql --default-character-set=utf8mb4 -uacademia -pacademia123 empleados_db < datos/semilla-empleados.sql`

## Endpoints

| Verbo | Ruta | Qué hace |
|---|---|---|
| GET | `/api/empleados?page=0&size=10&sort=id,asc` | Lista por páginas |
| GET | `/api/empleados/{id}` | Un empleado (404 si no existe) |
| POST | `/api/empleados` | Crea (201 + Location; 400 datos inválidos; 409 email repetido) |
| PUT | `/api/empleados/{id}` | Modifica (200; 400; 404; 409) |
| DELETE | `/api/empleados/{id}` | Borra (204; 404) |
| GET | `/api/empleados/buscar?departamento=&texto=&activo=&salarioMinimo=&salarioMaximo=` | Búsqueda con filtros opcionales, por páginas |
| GET | `/api/empleados/departamento/{departamento}` | Los de un departamento, por apellidos |
| GET | `/api/empleados/salarios?minimo=&maximo=` | Los de un rango de salario, del mayor al menor |

## Arquitectura

`EmpleadoController` (HTTP) → `EmpleadoService` (reglas) → `EmpleadoRepository` (Spring Data JPA) → MySQL.
Los datos entran como `EmpleadoRequest` y salen como `EmpleadoResponse` / `PaginaResponse`; los errores
salen como `ProblemDetail` desde `ManejadorErrores`.

## Evidencia

| Día | Archivos |
|---|---|
| Jueves 24 — entorno | `evidencia/dia1/entorno.txt` · `compose-ps.txt` · `describe-empleados.txt` |
| Viernes 25 — CRUD | `evidencia/dia2/crud.txt` · `mysql-select.txt` · `probar-crud.sh` |
| Sábado 26 — búsquedas y paginación | `evidencia/dia3/busquedas.txt` · `persistencia.txt` · `probar-busquedas.sh` |

## Qué aprendí y qué me costó

La creacion de este proyecto ha estado cargada de aprendizaje, toda la parte de docker, del funcionamiento del CRUD en la
parte del backend, el ver como se conecta cada parte para poder llevar todo el proceso del frontend, de la base de datos,
de la documentacion, todo ha sido nuevo para mi, por otro lado el llevar el control de todos los archivos gracias a tener
una jerarquia clara de las carpetas me ha ayudado a que sea mas sencillo de ubicar donde se tienen que realizar los cambios
y tambien con esto entender que hace cada archivo, desde los .java, pasando por el yaml, el xml, etc. Por otro lado, 
considero que la parte mas complicada fue justo entender como se construye toda la app, aunque esto lo veo normal por
la gran cantidad de archivos que tenemos.