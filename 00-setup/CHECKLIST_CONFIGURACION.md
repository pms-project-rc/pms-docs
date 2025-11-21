# ✅ Checklist de Configuración Inicial - PMS Project

**Desarrollador**: ___________________________  
**Fecha**: ___________________________

---

## 📥 Instalaciones Previas

- [ ] Git instalado
- [ ] Docker Desktop instalado y corriendo
- [ ] Python 3.12 instalado
- [ ] Node.js 20+ instalado
- [ ] Visual Studio Code instalado (opcional)

---

## 📂 Clonar Repositorios

```bash
mkdir pms-project && cd pms-project
```

- [ ] `git clone https://github.com/pms-project-rc/pms-backend.git`
- [ ] `git clone https://github.com/pms-project-rc/pms-frontend.git`
- [ ] `git clone https://github.com/pms-project-rc/pms-infra.git`
- [ ] `git clone https://github.com/pms-project-rc/pms-docs.git`

---

## 🐳 Docker (Base de Datos)

```bash
cd pms-infra
docker compose up -d
docker compose ps  # Verificar que estén corriendo
```

- [ ] PostgreSQL corriendo (puerto 5432)
- [ ] pgAdmin corriendo (puerto 5050)
- [ ] Acceso a http://localhost:5050 funciona

---

## 🐍 Backend (Python)

```bash
cd ../pms-backend
python -m venv venv
# Windows: .\venv\Scripts\Activate.ps1
# Mac/Linux: source venv/bin/activate
pip install -r requirements.txt
```

- [ ] Entorno virtual creado
- [ ] Dependencias instaladas (sin errores)
- [ ] Archivo `.env` creado con configuración
- [ ] `alembic history` funciona

**Probar backend:**
```bash
uvicorn app.main:app --reload
```

- [ ] Backend inicia sin errores
- [ ] http://localhost:8000/docs muestra Swagger UI

---

## ⚛️ Frontend (React)

```bash
cd ../pms-frontend
npm install
```

- [ ] `node_modules` instalado correctamente
- [ ] Archivo `.env.local` creado con `VITE_API_URL`

**Probar frontend:**
```bash
npm run dev
```

- [ ] Frontend inicia sin errores
- [ ] http://localhost:5173 muestra la aplicación

---

## 🎯 Verificaciones Finales

### Base de Datos
```bash
docker exec -it postgres psql -U pms_user -d pms_db -c "\dt"
```

- [ ] Veo 19 tablas listadas

### Conectividad
- [ ] Backend conecta a PostgreSQL (sin errores en consola)
- [ ] Frontend conecta a Backend (probar llamada API)

---

## 🛠️ Comandos Guardados para Referencia

### Iniciar todo:
```bash
# Terminal 1 - Docker
cd pms-infra && docker compose up -d

# Terminal 2 - Backend
cd pms-backend
source venv/bin/activate  # o .\venv\Scripts\Activate.ps1
uvicorn app.main:app --reload

# Terminal 3 - Frontend
cd pms-frontend
npm run dev
```

### Detener todo:
```bash
# Backend: Ctrl+C en terminal
# Frontend: Ctrl+C en terminal
# Docker:
cd pms-infra && docker compose down
```

---

## ❌ Problemas Encontrados

Anota aquí cualquier error que tuviste y cómo lo resolviste:

1. _______________________________________________________________
   Solución: _______________________________________________________

2. _______________________________________________________________
   Solución: _______________________________________________________

3. _______________________________________________________________
   Solución: _______________________________________________________

---

## ✅ ¡Configuración Completada!

**Fecha de finalización**: ___________________________  
**Tiempo total**: ___________ minutos  
**Todo funciona correctamente**: [ ] SÍ  [ ] NO

---

**Si marcaste "NO"**, contacta a:
- Tech Lead en Slack: #pms-development
- O crea un issue en GitHub

**Si marcaste "SÍ"**, ¡ya puedes empezar a desarrollar! 🚀
