# 🚀 Guía de Inicio para Desarrolladores - PMS Project

> **Objetivo**: Configurar tu entorno local para empezar a desarrollar en el proyecto PMS (Parking Management System)

---

## 📋 Prerrequisitos

Antes de empezar, asegúrate de tener instalado:

- ✅ **Git** - [Descargar aquí](https://git-scm.com/downloads)
- ✅ **Docker Desktop** - [Descargar aquí](https://www.docker.com/products/docker-desktop/)
- ✅ **Python 3.12** - [Descargar aquí](https://www.python.org/downloads/)
- ✅ **Node.js 20+** - [Descargar aquí](https://nodejs.org/)
- ✅ **Visual Studio Code** (recomendado) - [Descargar aquí](https://code.visualstudio.com/)

---

## 🎯 Paso 1: Clonar los Repositorios

El proyecto está dividido en 4 repositorios separados:

```bash
# Crear carpeta del proyecto
mkdir pms-project
cd pms-project

# Clonar todos los repositorios
git clone https://github.com/pms-project-rc/pms-backend.git
git clone https://github.com/pms-project-rc/pms-frontend.git
git clone https://github.com/pms-project-rc/pms-infra.git
git clone https://github.com/pms-project-rc/pms-docs.git
```

**Estructura resultante:**
```
pms-project/
├── pms-backend/    ← API con FastAPI + Python
├── pms-frontend/   ← UI con React + TypeScript
├── pms-infra/      ← Docker, PostgreSQL, configuración
└── pms-docs/       ← Documentación del proyecto
```

---

## 🐳 Paso 2: Levantar Docker (Base de Datos)

### **2.1 Iniciar Docker Desktop**
1. Abre Docker Desktop
2. Espera a que diga "Docker is running"

### **2.2 Levantar los servicios**

```bash
# Entrar a la carpeta de infraestructura
cd pms-infra

# Levantar todos los servicios en background
docker compose up -d
```

**¿Qué hace esto?**
- Crea contenedor de PostgreSQL (puerto 5432)
- Crea contenedor de pgAdmin (puerto 5050)
- Crea la base de datos `pms_db`
- Ejecuta el script `create_tables.sql` (19 tablas)
- Inserta datos de prueba

### **2.3 Verificar que funciona**

```bash
# Ver contenedores corriendo
docker compose ps

# Deberías ver:
# postgres    Up      5432/tcp
# pgadmin     Up      5050/tcp
```

### **2.4 Acceder a pgAdmin (Opcional)**

1. Abrir navegador en: http://localhost:5050
2. Login:
   - Email: `admin@pms.com`
   - Password: `admin123`
3. Agregar servidor PostgreSQL:
   - Host: `postgres`
   - Port: `5432`
   - Database: `pms_db`
   - Username: `pms_user`
   - Password: `pms_password`

---

## 🐍 Paso 3: Configurar el Backend (Python + FastAPI)

### **3.1 Entrar a la carpeta del backend**

```bash
cd ../pms-backend
```

### **3.2 Crear entorno virtual**

**En Windows (PowerShell):**
```powershell
python -m venv venv
.\venv\Scripts\Activate.ps1
```

**En Windows (CMD):**
```cmd
python -m venv venv
venv\Scripts\activate.bat
```

**En Mac/Linux:**
```bash
python3 -m venv venv
source venv/bin/activate
```

**¿Cómo saber que funcionó?**
Deberías ver `(venv)` al inicio de tu terminal:
```
(venv) C:\Users\tu-usuario\pms-project\pms-backend>
```

### **3.3 Instalar dependencias**

```bash
pip install -r requirements.txt
```

Esto instalará:
- FastAPI (framework web)
- SQLAlchemy (ORM para base de datos)
- Alembic (migraciones de BD)
- Pydantic (validación de datos)
- uvicorn (servidor ASGI)
- psycopg2 (driver de PostgreSQL)
- Y más...

**Tiempo estimado**: 2-3 minutos

### **3.4 Crear archivo de variables de entorno**

```bash
# Copiar el template
cp .env.example .env
```

**Editar `.env` con estos valores:**
```env
# Base de datos
DATABASE_URL=postgresql+asyncpg://pms_user:pms_password@localhost:5432/pms_db

# JWT (autenticación)
SECRET_KEY=your-super-secret-key-change-this-in-production
ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=30

# Configuración del servidor
DEBUG=True
PORT=8000
```

### **3.5 Verificar conexión a la base de datos**

```bash
# Ejecutar un script de prueba
python -c "from sqlalchemy import create_engine; engine = create_engine('postgresql://pms_user:pms_password@localhost:5432/pms_db'); print('✅ Conexión exitosa!' if engine.connect() else '❌ Error')"
```

Deberías ver: `✅ Conexión exitosa!`

### **3.6 (Opcional) Ejecutar migraciones de Alembic**

```bash
# Ver estado actual
alembic current

# Ver historial de migraciones
alembic history

# Aplicar todas las migraciones
alembic upgrade head
```

**Nota**: Por ahora las tablas ya están creadas por el script SQL de Docker, pero en el futuro usarás Alembic para cambios incrementales.

---

## ⚛️ Paso 4: Configurar el Frontend (React + TypeScript)

### **4.1 Entrar a la carpeta del frontend**

```bash
cd ../pms-frontend
```

### **4.2 Instalar dependencias de Node.js**

```bash
npm install
```

Esto instalará:
- React 18
- TypeScript
- Vite (build tool)
- React Router
- TailwindCSS
- Y más...

**Tiempo estimado**: 3-5 minutos

### **4.3 Crear archivo de variables de entorno**

```bash
# Copiar el template
cp .env.example .env
```

**Editar `.env.local` con estos valores:**
```env
VITE_API_URL=http://localhost:8000
VITE_APP_NAME=PMS - Parking Management System
```

---

## ✅ Paso 5: Verificar que Todo Funciona

### **5.1 Verificar la base de datos**

```bash
# Conectarse a PostgreSQL
docker exec -it postgres psql -U pms_user -d pms_db

# Dentro de PostgreSQL, ejecutar:
\dt    # Listar todas las tablas (deberías ver 19)
\q     # Salir
```

### **5.2 Iniciar el backend**

```bash
cd pms-backend

# Activar entorno virtual (si no está activo)
# Windows: .\venv\Scripts\Activate.ps1
# Mac/Linux: source venv/bin/activate

# Iniciar servidor
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

**Deberías ver:**
```
INFO:     Uvicorn running on http://0.0.0.0:8000 (Press CTRL+C to quit)
INFO:     Started reloader process
INFO:     Started server process
INFO:     Waiting for application startup.
INFO:     Application startup complete.
```

**Abrir navegador en:** http://localhost:8000/docs

Deberías ver la **documentación interactiva de la API** (Swagger UI) ✅

### **5.3 Iniciar el frontend** (en otra terminal)

```bash
cd pms-frontend

# Iniciar servidor de desarrollo
npm run dev
```

**Deberías ver:**
```
  VITE v5.x.x  ready in 500 ms

  ➜  Local:   http://localhost:5173/
  ➜  Network: use --host to expose
```

**Abrir navegador en:** http://localhost:5173

Deberías ver la **aplicación React** corriendo ✅

---

## 🎉 ¡Todo Listo!

Si llegaste hasta aquí, tu entorno está **100% configurado**.

### **Servicios corriendo:**

| Servicio | URL | Descripción |
|----------|-----|-------------|
| **Backend API** | http://localhost:8000 | FastAPI con endpoints REST |
| **API Docs (Swagger)** | http://localhost:8000/docs | Documentación interactiva |
| **Frontend** | http://localhost:5173 | Aplicación React |
| **pgAdmin** | http://localhost:5050 | Administrador de PostgreSQL |
| **PostgreSQL** | localhost:5432 | Base de datos |

---

## 🛠️ Comandos Útiles del Día a Día

### **Docker (Base de Datos)**

```bash
# Iniciar servicios
docker compose up -d

# Detener servicios
docker compose down

# Ver logs
docker compose logs -f postgres

# Reiniciar solo PostgreSQL
docker compose restart postgres

# Ver contenedores corriendo
docker compose ps
```

### **Backend (Python)**

```bash
# Activar entorno virtual
source venv/bin/activate         # Mac/Linux
.\venv\Scripts\Activate.ps1      # Windows

# Iniciar servidor (con hot-reload)
uvicorn app.main:app --reload

# Ejecutar tests
pytest

# Ver migraciones de Alembic
alembic history

# Crear nueva migración
alembic revision --autogenerate -m "descripción del cambio"

# Aplicar migraciones
alembic upgrade head

# Revertir última migración
alembic downgrade -1
```

### **Frontend (React)**

```bash
# Iniciar servidor de desarrollo
npm run dev

# Crear build de producción
npm run build

# Ejecutar tests
npm run test

# Linter (revisar errores)
npm run lint
```

### **Git (Control de Versiones)**

```bash
# Ver estado
git status

# Crear rama para tu feature
git checkout -b feature/nombre-de-tu-feature

# Ver cambios
git diff

# Agregar cambios
git add .

# Commit
git commit -m "feat: descripción del cambio"

# Subir a GitHub
git push origin feature/nombre-de-tu-feature

# Actualizar desde main
git pull origin main
```

---

## 📚 Recursos Importantes

### **Documentación del Proyecto**

- 📁 `pms-docs/01-definicion-proyecto/` - Especificaciones y requerimientos
- 📁 `pms-docs/02-arquitectura/` - Diagramas de arquitectura
- 📁 `pms-backend/alembic/README.md` - Guía de Alembic
- 📁 `pms-backend/STRUCTURE.md` - Estructura del código

### **Documentación Oficial de Tecnologías**

- [FastAPI](https://fastapi.tiangolo.com/) - Framework del backend
- [SQLAlchemy](https://docs.sqlalchemy.org/en/20/) - ORM
- [Alembic](https://alembic.sqlalchemy.org/) - Migraciones
- [React](https://react.dev/) - Framework del frontend
- [Vite](https://vitejs.dev/) - Build tool
- [PostgreSQL](https://www.postgresql.org/docs/) - Base de datos

---

## ❓ Troubleshooting (Problemas Comunes)

### **Error: "Docker daemon is not running"**
- Solución: Abre Docker Desktop y espera a que inicie completamente

### **Error: "Port 5432 is already in use"**
- Solución: Tienes otro PostgreSQL corriendo localmente
  ```bash
  # Windows: Detener servicio de PostgreSQL
  net stop postgresql-x64-15
  
  # O cambiar puerto en docker-compose.yml:
  ports:
    - "5433:5432"  # Usa 5433 en lugar de 5432
  ```

### **Error: "Module not found" en Python**
- Solución: Asegúrate de tener el entorno virtual activado
  ```bash
  # Verifica que diga (venv) en tu terminal
  pip list  # Ver paquetes instalados
  ```

### **Error: "ENOENT: no such file or directory" en npm**
- Solución: Borra `node_modules` y reinstala
  ```bash
  rm -rf node_modules package-lock.json
  npm install
  ```

### **Error: Alembic no puede conectarse a la BD**
- Solución: Verifica que PostgreSQL esté corriendo
  ```bash
  docker compose ps
  # Si no está corriendo:
  docker compose up -d postgres
  ```

---

## 👥 Flujo de Trabajo en Equipo

### **1. Antes de empezar a trabajar cada día**

```bash
# Actualizar código
git pull origin main

# Actualizar dependencias Python (si hubo cambios)
pip install -r requirements.txt

# Actualizar dependencias Node (si hubo cambios)
npm install

# Aplicar migraciones nuevas
alembic upgrade head
```

### **2. Al desarrollar una nueva feature**

```bash
# Crear rama
git checkout -b feature/HU-001-registro-entrada

# Hacer tus cambios...

# Commit frecuentes
git add .
git commit -m "feat: implement vehicle entry registration"

# Push a tu rama
git push origin feature/HU-001-registro-entrada

# Crear Pull Request en GitHub
```

### **3. Convenciones de Commits**

Usamos [Conventional Commits](https://www.conventionalcommits.org/):

```bash
feat: nueva funcionalidad
fix: corrección de bug
docs: cambios en documentación
style: formato de código (sin cambios lógicos)
refactor: refactorización de código
test: agregar o modificar tests
chore: tareas de mantenimiento
```

**Ejemplos:**
```bash
git commit -m "feat: add vehicle entry endpoint"
git commit -m "fix: correct parking cost calculation"
git commit -m "docs: update API documentation"
```

---

## 🚀 ¡Ya Puedes Empezar a Desarrollar!

### **Próximos Pasos Según tu Rol:**

#### **Dev A (Backend - Arquitectura)**
- ✅ Implementar casos de uso (Use Cases)
- ✅ Crear repositorios (Repository pattern)
- ✅ Definir entidades de dominio

#### **Dev B (Backend - Autenticación)**
- ⏳ Implementar JWT
- ⏳ Crear endpoints de login/logout
- ⏳ Middleware de autenticación

#### **Dev C (Backend - Historias de Usuario)**
- ⏳ HU-001: Registro de entrada
- ⏳ HU-002: Registro de salida
- ⏳ HU-003: Asignación de lavado

#### **Dev D (Frontend)**
- ⏳ Configurar rutas (React Router)
- ⏳ Crear componentes base
- ⏳ Implementar autenticación

---

## 📞 Soporte

Si tienes problemas, contacta a:
- **Tech Lead**: [Nombre]
- **Slack**: #pms-development
- **GitHub Issues**: Crea un issue en el repo correspondiente

---

**Última actualización**: Noviembre 2025  
**Versión**: 1.0.0  
**Mantenido por**: Equipo PMS
