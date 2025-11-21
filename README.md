# Documentación del Proyecto PMS (Parking Management System)

Bienvenido al repositorio central de documentación del **PMS**. Este proyecto es una plataforma integral para la gestión operativa y administrativa de negocios de parqueadero y lavado de vehículos.

## 📚 Estructura de la Documentación

La documentación está organizada en los siguientes módulos para facilitar su consulta:

### 📂 01-definicion-proyecto
Documentos de alto nivel sobre qué es el proyecto y qué debe hacer.
- [Especificaciones Generales del Producto](01-definicion-proyecto/especificacionesGeneralesProducto.md): **Documento principal** con todas las especificaciones, historias de usuario y reglas de negocio.
- [Tarifas y Precios](01-definicion-proyecto/tarifas-y-precios.md): Tablas completas de tarifas para parqueadero y servicios de lavado (4 tipos de vehículos).
- [Aclaraciones de Especificaciones](01-definicion-proyecto/aclaraciones-especificaciones.md): Q&A detallado sobre reglas de negocio, cálculos, bonos, exenciones, etc.
- [Visión y Alcance](01-definicion-proyecto/vision-y-alcance.md): Objetivos, justificación y límites del proyecto.
- [Historias de Usuario](01-definicion-proyecto/historias-de-usuario.md): Requerimientos funcionales detallados por rol.
- [Reglas de Negocio](01-definicion-proyecto/reglas-de-negocio.md): Políticas invariantes del dominio (tarifas, bonos, estados).
- [Glosario del Dominio](01-definicion-proyecto/glosario.md): Diccionario de términos ubicuos (DDD).
- [Plan de Trabajo](01-definicion-proyecto/plan-de-trabajo.md): Asignación de historias de usuario por desarrollador.

### 📂 02-arquitectura
Detalles técnicos para los desarrolladores.
- [DB Diagram (DBML)](02-arquitectura/dbdiagram.dbml): **Schema completo de base de datos** con 16+ tablas (formato DBML).
- [UML Diagram (PlantUML)](02-arquitectura/umldiagram.plantuml): Diagrama de clases y relaciones entre entidades.
- [Visión General y Stack](02-arquitectura/vision-general.md): Diagrama de componentes y tecnologías (FastAPI, React, Postgres).
- [Modelo de Datos](02-arquitectura/modelo-datos.md): Esquemas de base de datos y entidades.
- [APIs y Contratos](02-arquitectura/apis.md): Definición de endpoints y comunicación.

### 📂 03-manuales
Guías de uso y operación.
- [Guía de Despliegue](03-manuales/despliegue.md): Cómo levantar el proyecto con Docker Compose.
- [Manual de Usuario](03-manuales/usuario.md): Guía para el Administrador Operativo y Global.

---

## 🚀 Enlaces Rápidos a Otros Repositorios

- **Backend**: [pms-backend](../pms-backend/) - API REST con FastAPI
- **Frontend**: [pms-frontend](../pms-frontend/) - Aplicación web con React
- **Infraestructura**: [pms-infra](../pms-infra/) - Docker Compose y scripts

### Documentación Principal del Proyecto (Raíz)
- [README Principal](../README.md) - Visión general del proyecto completo
- [Quick Start](../QUICK_START.md) - Guía de inicio rápido con Docker
- [Implementation Summary](../IMPLEMENTATION_SUMMARY.md) - Resumen de implementación
- [Development Guide](../DEVELOPMENT_GUIDE.md) - Guía de desarrollo para el equipo

---

## 🛠 Stack Tecnológico Resumido

| Componente | Tecnología |
|------------|------------|
| **Backend** | Python 3.12 + FastAPI + SQLAlchemy |
| **Frontend** | React 18 + TypeScript + Vite + Tailwind CSS |
| **State Management** | Redux Toolkit + React Query |
| **Base de Datos** | PostgreSQL 15 |
| **Infraestructura** | Docker + Docker Compose |
| **Testing** | Pytest (Backend), Vitest (Frontend) |
| **Arquitectura** | Clean Architecture + DDD + Hexagonal |

---

## 📊 Modelo de Datos

### Tablas Principales (16+)

1. **global_admins** - Administradores globales
2. **operational_admins** - Administradores operativos
3. **washers** - Lavadores
4. **vehicles** - Vehículos registrados
5. **parking_records** - Registros de parqueadero
6. **washing_services** - Servicios de lavado
7. **shifts** - Turnos de trabajo
8. **expenses** - Gastos operativos
9. **bonuses** - Bonos de lavadores
10. **vouchers** - Vales de descuento
11. **rates** - Tarifas
12. **monthly_subscriptions** - Mensualidades
13. **agreements** - Convenios empresariales
14. **fleet_vehicles** - Vehículos de flotas
15. **audit_logs** - Log de auditoría
16. **notifications** - Notificaciones del sistema
17. **financial_reports** - Reportes financieros
18. **business_config** - Configuración del negocio
19. **password_reset_tokens** - Tokens de recuperación de contraseña

Ver schema completo: [dbdiagram.dbml](02-arquitectura/dbdiagram.dbml)

---

## 💰 Tarifas Resumidas

### Parqueadero (0-6 horas - Por Minuto)
| Vehículo | Tarifa/Min |
|----------|------------|
| Carro    | $80        |
| Moto     | $50        |
| Camión   | $120       |
| Bicicleta| $30        |

### Parqueadero (6-12 horas - Tarifa Plana)
| Vehículo | Tarifa |
|----------|--------|
| Carro    | $20,000|
| Moto     | $10,000|
| Camión   | $35,000|
| Bicicleta| $5,000 |

### Mensualidades
| Vehículo | Precio/Mes |
|----------|------------|
| Carro    | $170,000   |
| Moto     | $70,000    |
| Camión   | $300,000   |
| Bicicleta| $40,000    |

Ver todas las tarifas: [tarifas-y-precios.md](01-definicion-proyecto/tarifas-y-precios.md)

---

## 👥 Equipo y Asignación de Módulos

### Dev A
**Backend:** Auth, Users, Password Recovery  
**Frontend:** Login, Admin Layout, User Management

### Dev B
**Backend:** Parking, Washing, Shifts  
**Frontend:** Parking Module, Washing Module, Dashboard Operativo

### Dev C
**Backend:** Rates, Subscriptions, Agreements  
**Frontend:** Rates Config, Subscriptions, Agreements

### Dev D
**Backend:** Reports, Analytics, Audit Logs  
**Frontend:** Reports, Charts, Dashboard Global

---

## 📈 Estado del Proyecto

### ✅ Fase 1 - Completada
- [x] Análisis de especificaciones
- [x] Diseño de base de datos (16+ tablas)
- [x] Documentación de tarifas y reglas de negocio

### ✅ Fase 2 - Completada
- [x] Estructura de carpetas (backend, frontend, infra)
- [x] Configuración de Docker Compose
- [x] Archivos de configuración (package.json, requirements.txt, etc.)
- [x] README y documentación completa

### ⏳ Fase 3 - En Progreso
- [ ] Implementación de entidades de dominio
- [ ] Repositorios y casos de uso
- [ ] Endpoints de API
- [ ] Componentes de UI

---

## 🔗 Enlaces Útiles

- **Visualización DB**: https://dbdiagram.io (importar dbdiagram.dbml)
- **API Docs (local)**: http://localhost:8000/docs
- **Frontend (local)**: http://localhost:5173

---

**Licencia**: MIT  
**Última Actualización**: 2024
