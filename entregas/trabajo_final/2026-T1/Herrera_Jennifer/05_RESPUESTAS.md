# Preguntas de Comprensión

## 1. Infraestructura

Si la app intenta conectarse a PostgreSQL antes de que la base este lista, el arranque puede fallar por timeout o por rechazo de conexion.

La forma correcta de mitigarlo en Docker Compose es:

- usar `healthcheck` en PostgreSQL
- usar `depends_on` con `condition: service_healthy`
- si hiciera falta, añadir reintentos desde la propia aplicación

## 2. JPA

`findAll()` puede ser lento con 100000 registros porque carga demasiadas filas en memoria, aumenta el tiempo y empeora el consumo de RAM.

La alternativa correcta es usar paginación con `Pageable`, filtros y, si hace falta, DTOs o proyecciones para devolver solo la información necesaria.

## 3. API REST

Un error `400 Bad Request` significa que el cliente envío datos inválidos. En este proyecto sería, por ejemplo, crear un usuario con email inválido o nombre vacío.

<img width="1824" height="2304" alt="error400" src="https://github.com/user-attachments/assets/6ed6ae7a-fd1a-4146-816b-a7ef1eab5c5c" />


Un error `500 Internal Server Error` significa que el fallo ocurre dentro del servidor. En este proyecto sería un error inesperado no controlado en tiempo de ejecución.

<img width="1900" height="2668" alt="error500" src="https://github.com/user-attachments/assets/5759c527-6094-4faa-9859-3f15823ee026" />



## 4. Escalabilidad

Si la aplicación pasara de 100 a 10000 usuarios cambiaría:

- separar la API y PostgreSQL en despliegues distintos
- externalizar configuración y secretos
- monitorizar CPU, memoria y tiempos de respuesta
- optimizar queries, indices y paginación
- valorar caché para consultas frecuentes
