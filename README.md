# Documentación del Proyecto PMS (Parking Management System)

Bienvenido al repositorio central de documentación del **PMS**. Este proyecto es una plataforma integral para la gestión operativa y administrativa de negocios de parqueadero y lavado de vehículos.

## 📚 Estructura de la Documentación

La documentación está organizada en los siguientes módulos para facilitar su consulta:

### 📂 01-definicion-proyecto
Documentos de alto nivel sobre qué es el proyecto y qué debe hacer.
- [Visión y Alcance](01-definicion-proyecto/vision-y-alcance.md): Objetivos, justificación y límites del proyecto.
- [Historias de Usuario](01-definicion-proyecto/historias-de-usuario.md): Requerimientos funcionales detallados por rol.
- [Reglas de Negocio](01-definicion-proyecto/reglas-de-negocio.md): Políticas invariantes del dominio (tarifas, bonos, estados).
- [Glosario del Dominio](01-definicion-proyecto/glosario.md): Diccionario de términos ubicuos (DDD).
- [Plan de Trabajo](01-definicion-proyecto/plan-de-trabajo.md): Asignación de historias de usuario por desarrollador.

### 📂 02-arquitectura
Detalles técnicos para los desarrolladores.
- [Visión General y Stack](02-arquitectura/vision-general.md): Diagrama de componentes y tecnologías (FastAPI, React, Postgres).
- [Modelo de Datos](02-arquitectura/modelo-datos.md): Esquemas de base de datos y entidades.
- [APIs y Contratos](02-arquitectura/apis.md): Definición de endpoints y comunicación.

### 📂 03-manuales
Guías de uso y operación.
- [Guía de Despliegue](03-manuales/despliegue.md): Cómo levantar el proyecto con Docker Compose.
- [Manual de Usuario](03-manuales/usuario.md): Guía para el Administrador Operativo y Global.

---

## 🛠 Stack Tecnológico Resumido

| Componente | Tecnología |
|------------|------------|
| **Backend** | Python 3.12 + FastAPI |
| **Frontend** | React 18 + Tailwind CSS |
| **Base de Datos** | PostgreSQL 15 |
| **Infraestructura** | Docker + Docker Compose |

## 👥 Equipo y Roles
- **Dev A (Juan Camilo)**: Líder Técnico, Financiero, Analítica.
- **Dev B (Juan Esteban)**: Operativo, Tarifas, Mensualidades.
- **Dev C (Jaime Darley)**: Frontend, UI/UX.
- **Dev D (Jeremy Lee)**: Soporte, Módulos Auxiliares.
