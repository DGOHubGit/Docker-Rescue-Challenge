# ACI606 - Docker Rescue Challenge

Aplicación multi-servicio: Frontend (Nginx) → Backend (FastAPI) → Base de datos (PostgreSQL).

## Requisitos
- Docker y Docker Compose

## 1. Configuración
```bash
cp .env.example .env
```
Edita `.env` si quieres cambiar usuario, clave, base de datos o puerto.

## 2. Construir e iniciar
```bash
docker compose up --build -d
docker compose ps
```
Los tres servicios deben aparecer `Up`, y `db` como `healthy`.

## 3. Verificar
```bash
curl -s http://localhost:8080/api/health
curl -s -X POST http://localhost:8080/api/items -H "Content-Type: application/json" -d '{"name":"prueba"}'
curl -s http://localhost:8080/api/items
```
También se puede abrir `http://localhost:8080` en el navegador (en Codespaces, desde la pestaña Puertos).

## 4. Logs y diagnóstico
```bash
docker compose logs backend
docker compose logs frontend
docker compose logs db
docker compose config
```

## 5. Detener
```bash
docker compose down
```
Los datos se conservan en el volumen `pgdata`.

## 6. Recrear
```bash
docker compose down
docker compose up --build -d
```
Los datos persisten tras recrear los contenedores.

## 7. Borrado completo (ELIMINA LOS DATOS)
```bash
docker compose down -v
```

## Arquitectura
- `frontend`: Nginx en el puerto 80 interno, publicado en `${FRONTEND_PORT}`; hace proxy de `/api/` hacia `backend:8000`.
- `backend`: FastAPI en el puerto 8000, se conecta a `db` mediante `DATABASE_URL`.
- `db`: PostgreSQL 16 con volumen nombrado `pgdata` y healthcheck `pg_isready`.

## Diagrama de arquitectura

```
Navegador -> frontend (Nginx :80, publicado 8080) -> backend (FastAPI :8000) -> db (PostgreSQL, volumen pgdata)
```

El diagrama detallado de la arquitectura inicial y final está en el informe técnico (secciones 2 y 6).
