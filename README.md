# Plataforma de Evaluación Ciudadana (PEC)

Este proyecto permite levantar fácilmente toda la infraestructura de la aplicación **PEC** mediante Docker Compose usando imágenes públicas alojadas en **GitHub Container Registry (GHCR)**.

---

## 🚀 Instrucciones rápidas

### 1. Clonar el repositorio
```bash
git clone https://github.com/maxxired/pec.git
cd pec
```

### 2. Configurar variables de entorno
Cada servicio tiene su propio archivo `.env` dentro de su carpeta. Copia los archivos de ejemplo y completa tus valores:

```bash
cp PEC-Back/.env.example.example PEC-Back/.env.example
cp PECI-WebService/app/.env.example.example PECI-WebService/app/.env.example
cp PECI-WebService/scheduler/.env.example.example PECI-WebService/scheduler/.env.example
cp PECI-WebService/workers/.env.example.example PECI-WebService/workers/.env.example
cp .env.example.example .env.example   # si existe en la raíz
```

> 🔒 No compartas los archivos `.env` reales en el repositorio. Solo incluye los `.env.example`.

---

### 3. Ejecutar el stack
```bash
docker compose -f docker-compose.public.yml up -d
```

Esto iniciará:
- PostgreSQL (db)
- Redis (cache)
- RabbitMQ (mensajería)
- API principal (.NET)
- Frontend (React)
- Microservicio API-LIA (Python FastAPI)
- Scheduler (Python)
- Worker de sincronización (Python)

---

### 4. Verificar que todo esté corriendo
```bash
docker ps
```
Deberías ver los contenedores activos y sus puertos:

| Servicio       | Puerto local | Descripción |
|----------------|---------------|--------------|
| frontend       | 8080          | Interfaz web |
| backend (.NET) | 7001          | API principal |
| apilia         | 8000          | Microservicio LIA |
| rabbitmq (UI)  | 15672         | Panel de mensajería |
| db             | 5435          | PostgreSQL |
| cache          | 6379          | Redis |

---

## 🧩 Estructura de archivos

```
.
├── docker-compose.yml
├── docker-compose.public.yml
├── PEC-Back/
│   ├── .env.example
├── PECI-WebService/
│   ├── app/
│   │   ├── .env.example
│   ├── scheduler/
│   │   ├── .env.example
│   └── workers/
│       ├── .env.example
└── .env.example
```

---

## 🧾 Variables de entorno por servicio

### Backend (.NET)
```
DB_USER=
DB_PASSWORD=
DB_NAME=
PORT=5432
ENVIRONMENT=Production
```

### Apilia (FastAPI)
```
DB_HOST=db
DB_PORT=5432
DB_NAME=
DB_USER=
DB_PASSWORD=
RABBIT_HOST=rabbitmq
RABBIT_PORT=5672
RABBIT_USER=
RABBIT_PASS=
REDIS_HOST=cache
REDIS_PORT=6379
```

### Scheduler
```
RABBIT_HOST=rabbitmq
RABBIT_PORT=5672
RABBIT_USER=
RABBIT_PASS=
CRON_EXPRESSION=*/5 * * * *
```

### Worker Sync
```
RABBIT_HOST=rabbitmq
RABBIT_PORT=5672
RABBIT_USER=
RABBIT_PASS=
QUEUE_KNOWLEDGE=knowledge
PDF_INPUT_DIR=/data/pdf
```

### Global (raíz)
```
RABBIT_USER=
RABBIT_PASS=
```

---

## 🐳 Información técnica

Todas las imágenes provienen de **GHCR.io/maxxired**:

| Servicio | Imagen GHCR |
|-----------|--------------|
| Backend (.NET) | ghcr.io/maxxired/pec-backend:0.1.0 |
| Frontend (React) | ghcr.io/maxxired/pec-frontend:0.1.0 |
| Base de datos | ghcr.io/maxxired/pec-db:0.1.0 |
| Microservicio Apilia | ghcr.io/maxxired/pec-apilia:0.1.0 |
| Scheduler | ghcr.io/maxxired/pec-scheduler:0.1.0 |
| Worker Sync | ghcr.io/maxxired/pec-worker-sync:0.1.0 |

Estas imágenes son **públicas**, por lo que no se requiere autenticación para usarlas.

---

## 💡 Consejos
- Mantén tus `.env` fuera del repositorio.
- Puedes usar `docker compose down` para detener todo.
---

