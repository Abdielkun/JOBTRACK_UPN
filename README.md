# JOBTRACK · Sprint 1

Sistema web para que estudiantes de últimos ciclos y egresados recientes de UPN Chorrillos organicen y den seguimiento a sus postulaciones laborales. Este código fue elaborado para el proyecto académico JOBTRACK.

## Alcance implementado

| Historia | Función |
| --- | --- |
| JT-01 | Registro, inicio y cierre de sesión; contraseña con hash y tokens con renovación |
| JT-02 | Nueva postulación con empresa, puesto, fecha, fuente y notas opcionales |
| JT-03 | Listado y detalle de postulaciones del propietario |
| JT-04 | Cambio de estado con historial fechado |
| JT-05 | Verificación del propietario en cada consulta y modificación del API |

El tablero muestra cifras calculadas a partir de las postulaciones del usuario. Los recordatorios, documentos, importación por URL y notificaciones quedan para sprints posteriores.

## Estructura

- `frontend/`: React, TypeScript y Vite.
- `backend/`: API REST con NestJS.
- `backend/migrations/`: esquema versionado de PostgreSQL.
- `docker-compose.yml`: base de datos para desarrollo local.

## Requisitos

Node.js 20.19 o superior, npm y PostgreSQL 17 (puedes iniciarlo con Docker Compose). No subas claves ni datos personales al repositorio.

## Ejecutar el Sprint 1 completo

1. Clona el repositorio y ejecuta `npm run install:all` desde la raíz.
2. Levanta PostgreSQL con `docker compose up -d db`.
3. Copia `backend/.env.example` a `backend/.env` y reemplaza `JWT_SECRET` por un valor aleatorio de al menos 32 caracteres. Ajusta `DATABASE_URL` si usas otra base.
4. Copia `frontend/.env.example` a `frontend/.env`.
5. Ejecuta `npm run db:migrate --prefix backend`.
6. En una terminal ejecuta `npm run dev --prefix backend`.
7. En otra terminal ejecuta `npm run dev --prefix frontend` y abre `http://localhost:5173`.

En el modo completo, los datos se guardan en PostgreSQL. Crea dos cuentas distintas para probar el aislamiento; un usuario solo debe ver sus propias postulaciones.

## Demo visual independiente

Ejecuta `npm run dev:demo --prefix frontend` para explorar el flujo sin API ni base de datos. El modo demo muestra datos ficticios y guarda los cambios en `localStorage` del navegador. La demo pública usa este modo, así que **no permite verificar autenticación, autorización ni persistencia en PostgreSQL**. Para esas pruebas se requiere el entorno completo.

Para compilar la demo estática: `npm run build --prefix frontend -- --mode demo`. La salida estará en `frontend/dist/`.

## API

| Método | Ruta | Uso |
| --- | --- | --- |
| POST | `/api/auth/register` | Registrar candidato |
| POST | `/api/auth/login` | Iniciar sesión |
| POST | `/api/auth/refresh` | Renovar token; cookie HttpOnly |
| POST | `/api/auth/logout` | Revocar sesión |
| GET | `/api/applications` | Listar las propias |
| POST | `/api/applications` | Crear una postulación |
| GET | `/api/applications/:id` | Ver detalle e historial propio |
| PATCH | `/api/applications/:id/status` | Cambiar estado de una postulación propia |

Las rutas de postulaciones requieren `Authorization: Bearer <accessToken>`. La API responde 404 a una solicitud de detalle o modificación de un registro ajeno; así evita revelar su existencia.

## Evidencia sugerida para PC5

Registra el commit presentado, la URL de la demo, capturas de alta e inicio de sesión, creación y cambio de estado, y una prueba con dos usuarios que demuestre que la cuenta B no accede a una postulación de A. La demo visual por sí sola no acredita los controles de la API.
